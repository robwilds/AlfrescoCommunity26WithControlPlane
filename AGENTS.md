# Alfresco Community 26.x — Agent Context

## Quick start

```bash
./start.sh                      # Control plane at http://localhost:9700 + opens browser
python3 mgr/server.py           # Control plane only (no auto-open)
docker compose up -d            # Start all 12 default services
docker compose up -d donotstart # Start profile-gated services too
```

## Architecture

- **Python backend** (`mgr/server.py`, 1343 lines): stdlib `http.server`, all Docker via `subprocess.run`. No framework.
- **JS frontend** (`mgr/static/index.html`, 2948 lines): vanilla JS, zero deps, dark/light CSS variables. Renders via `innerHTML`.
- **Compose files**: `docker-compose.yaml` (18 services) + `commons/base.yaml` (traefik routing labels). `mgr/update_compose.py` handles upstream template fetching.
- **MCP server**: container `wildsalfmcp` exposes SSE at `http://localhost:8000/sse`; configured in `.vscode/mcp.json`.

## Reference files

| File | Contents |
|------|----------|
| `CLAUDE.md` | Full API endpoint table, frontend state model, service listing |
| `README.md` | UI walkthrough, feature docs, tech notes |
| `DEMO_SCRIPT.md` | 14-scene video demo walkthrough |
| `MEMORY.md` | Development history and decisions |

Read `CLAUDE.md` first for API details; it has the canonical endpoint list.

## Docker service model

`docker-compose.yaml` defines 18 services; default `docker compose up -d` starts 12 non-profile-gated ones. Six services with `profiles: [donotstart]` are omitted by default:

- `content-app`, `control-center`, `open-webui`, `wildsalfmcp`, `email`, `webmail`

Services are discovered via `docker compose ps --format json` and `docker inspect` (project-name agnostic). Container name globals: `ALFRESCO_CONTAINER`, `SHARE_CONTAINER` in `mgr/server.py`.

**Install directories map to containers**: `installs/content/` → alfresco (`amps/`, `webapps/alfresco`); `installs/share/` → share (`amps_share/`, `webapps/share`).

## Gotchas

- **AMP install needs `--user root`**: MMT install target `webapps/WEB-INF` belongs to root, but the container runs as non-root. `docker exec --user root` is mandatory (see `do_install_amp()`).
- **MMT jar path is dynamic**: Resolved at runtime via `docker exec` glob of `/usr/local/tomcat/alfresco-mmt/*.jar` — image upgrades are picked up automatically; `DEFAULT_MMT_JAR` is only a fallback.
- **AMP lifecycle**: copy to container → MMT install → rename `.amp` → `.applied`. Uninstall runs MMT, removes `.applied` marker (or reverts to `.amp` if no local source), so the AMP shows Available again for reinstall.
- **JAR tracking**: Only JARs installed via the UI are removable. Persisted to `mgr/data/installed_jars.json` (`alfresco`/`share` arrays). `backfill_tracked_jars()` auto-detects already-deployed JARs on startup.
- **Start error reporting**: `do_start()` runs `docker compose up -d` as a batch, then inspects `docker compose ps --format json` post-attempt for per-service success/failure. Don't assume the whole batch failed if any single service reports error.
- **Background pull**: `docker compose pull` streams line-by-line via a background thread; state tracked in `_pull_state` dict protected by `_pull_lock`. Frontend polls `GET /api/pull-status` every 600ms.
- **Compose paths config**: `COMPOSE_PATHS` dict in `server.py` maps filenames to disk paths. Whitelist for `POST /api/compose` is `docker-compose.yaml` and `commons/base.yaml`.
- **Delete path validation**: `path.resolve().relative_to(installs_dir.resolve())` prevents directory traversal.

## Version sync & rollback

- `POST /api/compose` writes compose files after user-applied selection from a side-by-side diff (compared after stripping `docker.io/` prefix).
- Every write records an entry in `mgr/data/compose_rollback.json` (last 20) with `prevContent` and per-service `{service, before, after}` changes.
- `GET /api/rollback` lists history; `POST /api/rollback` restores one service or the whole snapshot. Rollback locates the `image:` line in the *current* file, so it stays correct even if the YAML is hand-edited between versions.

## Key implementation details

- **Refresh**: Auto-refresh every 5 s; accelerates to 1 s during pending start/stop/restart via `startFastRefreshUntil()`.
- **Alfresco health**: HTTP probe to `/alfresco/api/-default-/public/alfresco/versions/1/probes/-ready-`. Share health: HTTP probe to `/share`.
- **Quay.io login**: `docker login quay.io --password-stdin`. Images checked via `docker compose config --images` filtered to quay.io.
- **Properties editor**: Reads/writes `data/services/content/alfresco-global.properties` (bind-mounted into container).
- **File upload**: Base64 in JSON body (no multipart parser — `http.server` limitation).
- **CORS**: All responses include `Access-Control-Allow-Origin: *`.
