# Control Plane Demo Script

Record your screen at 1920×1080. Start with the control plane already open at http://localhost:9700 and Docker running with services started.

---

## Scene 1 — Landing Page (0:00 – 0:50)

**Visual**: Browser at localhost:9700 — dark theme UI fully loaded.

**Narration**:
> "This is the Alfresco Community 26.x Control Plane. It's a web-based dashboard for managing Docker services without using the command line. Zero dependencies — just Python stdlib on the backend and vanilla JavaScript on the frontend."

**Actions**:
- Slow hover over the Hyland-branded header showing "Alfresco Control Plane"
- Point to the **disclaimer banner** at top: "This demo utilizes a sample custom UI…"
- Point to the status badge: "X/Y running" — green when all running, red otherwise
- Point to **Start All / Stop All / Restart All** buttons — note they auto-disable: Start All disabled when all running, Stop All / Restart All disabled when none running
- Point to the **Refresh** button
- Point to the **theme toggle** (checkbox with Dark/Light label) in the header
- Point to the **?** button (restarts the guided tour on click)
- Point to the **🔄 Update Versions** button (syncs image versions from upstream Alfresco templates)
- Point to the **Open Alfresco ↗** link — appears whenever any service is running
- Point to the **Kibana ↗** link — appears when Kibana service is running

---

## Scene 2 — Services Panel (0:50 – 1:50)

**Visual**: Services table showing alfresco, share, then alphabetical services.

**Narration**:
> "The Services panel lists every service from our docker-compose file. Alfresco and Share always appear first, followed by the rest alphabetically. Services with a `profiles: [donotstart]` config get a badge and are excluded from the default start. Each service shows a green or red status dot, and we have Start, Stop, and Restart controls per service or globally."

**Actions**:
- Scroll slowly down the service list
- Hover over a profile-tagged service — point out the "profile: donotstart" badge
- Click the **Stop** button on a non-critical service (e.g., `postgres`)
- Wait for the toast: "postgres: stopped"
- Point to the status changing to red, global Stop All button disabling
- Click the **Start** button to bring it back up
- Toast: "postgres: started"
- Point to the **▶ Show Logs** toggle and the **Dozzle ↗** link on each service row
- When services are starting, point out the **animated pulsing dot** and "starting…" indicator that persists until the container reports healthy

---

## Scene 3 — Log Accordion & Dozzle (1:50 – 2:40)

**Visual**: Service row with ▶ Show Logs and Dozzle ↗ inline with service name.

**Narration**:
> "Each service has a built-in log viewer and integrates with Dozzle for real-time monitoring."

**Actions**:
- Click **▶ Show Logs** on the `alfresco` row
- Point to the toggle changing to **▼ Hide Logs**
- Point to the log content appearing below in monospace, showing timestamps with red highlighting on ERROR/FATAL lines
- Scroll the log content area
- Click **▶ Show Logs** on `share` to open multiple logs simultaneously
- Collapse both by clicking **▼ Hide Logs**
- Click the **Dozzle ↗** link on the `alfresco` service
- A new tab opens at http://localhost:9999 showing Alfresco logs streaming live
- Switch back to the control plane tab
- "You can monitor all containers in real time without leaving your browser."

---

## Scene 4 — Global Properties Editor (2:40 – 3:10)

**Visual**: The Alfresco service row with the "▶ Edit Global Properties" button.

**Narration**:
> "Alfresco has a built-in properties editor. You can modify `alfresco-global.properties` directly from the browser."

**Actions**:
- On the alfresco row, click **▶ Edit Global Properties**
- Point to the textarea editor showing the current properties file
- Make a small edit in the textarea
- Click **Save** — show the green "✓ Saved" status
- Narrate: "Changes take effect on restart — a prompt appears asking to restart the container"
- Click **Discard & Reload** to show the discard flow
- Click **▼ Properties** again then collapse it

---

## Scene 5 — Guided Tour (3:10 – 4:00)

**Visual**: The guided tour overlay appears, dimming the background and highlighting elements.

**Narration**:
> "On first visit, a built-in guided tour walks through every area of the UI. It highlights each panel in sequence with a description — no learning curve."

**Actions**:
- On a fresh browser (or click the **?** button to restart), the tour starts after 2 seconds
- Step 1 highlights the **Service Controls** — Start All / Stop All / Restart All with tooltip
- Click **OK** to advance
- Step 2 highlights the **Services Table** — per-service status and controls
- Step 3 highlights **Logs & Monitoring** — Show Logs and Dozzle ↗
- Step 4 highlights **File Management** — Upload, Install, Delete
- Step 5 highlights the **Installed Modules** — AMPs panel with installed + pending lists
- Step 6 highlights the **Library JARs** — WEB-INF/lib listing with Remove
- After step 6, the tour ends automatically
- "The tour runs once per browser tracked via localStorage. Click **?** anytime to restart it."

---

## Scene 6 — Uploading & Installing Files (4:00 – 5:15)

**Visual**: Available Files section, Content tab active.

**Narration**:
> "The Available Files section lets us manage AMPs and JARs. We can upload files directly through the browser, then install them into running containers."

**Actions**:
- Ensure the **Content** tab is active
- Click **Upload File** — the file picker opens
- Select a `.jar` or `.amp` file from a local folder (have one ready)
- Watch the button change to "Uploading..."
- Toast appears: "myfile.jar uploaded"
- Point to the file appearing in the list
- A `confirm()` dialog pops up: "Do you want to install it now?"
- **Click Cancel** to show the dialog, then narrate
- "If we click OK, it would install the file immediately. We'll show that next."

---

## Scene 7 — Installing & Deleting Files (5:15 – 6:00)

**Visual**: File list with Install AMP / Install JAR and Delete buttons.

**Narration**:
> "Each file has Install and Delete buttons. Buttons show '(done)' when the file is already installed. They're disabled when Docker isn't running, with a message indicating they will be enabled once Docker starts."

**Actions**:
- Click **Install JAR** on a `.jar` file
- Toast: "myfile.jar: copied"
- Point to the button showing "Install JAR (done)" — disabled state indicating already installed
- Switch to the **JARs** tab in the right panel
- "The JAR now appears in WEB-INF/lib — it will be live after the next restart."
- Click **Delete** on the same file
- Confirmation dialog: "WARNING: Deleting files can be dangerous! Are you sure you want to delete..."
- **Click OK**
- Toast: "myfile.jar deleted"
- "The file is removed from the directory."

---

## Scene 8 — AMPs Panel (6:00 – 6:50)

**Visual**: AMPs panel showing installed modules, available from installs/, and pending.

**Narration**:
> "The AMPs panel shows installed modules via MMT, available AMPs from the local installs directory, and what's pending in the container's amps dir ready to be installed."

**Actions**:
- Point to the **Installed** table — Title, Version, ID columns, each with a **Remove** button
- "Every installed AMP can be removed, even those baked into the image."
- Click **Remove** on an installed AMP
- Toast: "`module_id`: removed and available for reinstall"
- "The AMP is uninstalled and reappears in Available, ready to be reinstalled with one click."
- Point to the **Available (in installs/ directory)** section — lists AMPs with **Install AMP** buttons
- Point to the **Pending (in amps dir)** section — lists AMP files in the container's amps dir (filtered to exclude already-installed AMPs)
- Switch between **All Services**, **Alfresco**, and **Share** tabs
- "The All Services tab aggregates both containers."

---

## Scene 9 — JARs Panel (6:50 – 7:20)

**Visual**: JARs panel showing installed JARs, available from installs/, and all three tabs.

**Narration**:
> "The JARs panel gives us visibility into every JAR deployed in WEB-INF/lib and available in the local installs directory. Only JARs installed through the UI are removable — they're tracked in a local JSON file."

**Actions**:
- Point to the **Installed (WEB-INF/lib)** list with Remove buttons
- Point to the **Available (in installs/ directory)** section with Install buttons
- Switch between **All Services**, **Alfresco**, and **Share** tabs
- "The All Services tab shows both containers with service badges, just like AMPs does."
- Click **Remove** on a tracked JAR
- Toast: "myfile.jar: removed"
- "The JAR is deleted from the running container. Changes take effect on restart."

---

## Scene 10 — Batch Install (7:20 – 7:50)

**Visual**: Available Files section with the Install All button.

**Narration**:
> "The Install All button batch-installs all uninstalled JARs and AMPs in sequence, showing a live progress counter."

**Actions**:
- In the Content tab, point to the **Install All** button next to Upload File
- Click it — show the button text updating as a progress counter: "0/3", "1/3", "2/3", "3/3"
- Narrate: "Each file installs sequentially. Any errors are reported but don't stop the rest. A Restart Required prompt appears when done."

---

## Scene 11 — Wait Banner & Restart Prompt (7:50 – 8:40)

**Visual**: Alfresco starting up — wait banner with animated logo.

**Narration**:
> "While waiting for Alfresco to start or restart, an animated wait banner appears at the top with a spinning petal logo cycling through green, blue, orange, and yellow. When healthy, a prompt offers to open Alfresco."

**Actions**:
- Narrate the restart flow from Scene 4 (after properties save)
- Point to the **Restart Required** overlay: "The container needs to be restarted for the changes to take effect."
- Click **Restart Now**
- Show the animated wait banner with cycling petal colors
- "The banner disappears once the health probe passes or the Alfresco ready prompt appears."
- Point to the **Alfresco is ready** popup — "Open Alfresco" and "Not now" buttons
- "Click Open Alfresco to launch http://localhost:8080/alfresco in a new tab."

---

## Scene 12 — Quay.io Login & Image Pull (8:40 – 9:15)

**Visual**: Quay.io login overlay followed by the streaming pull output.

**Narration**:
> "When Start All is clicked and images need to be pulled from quay.io, a login overlay prompts for credentials. After login, a streaming pull output is shown."

**Actions**:
- Narrate: "If images are missing, the **Quay.io Login** overlay prompts for username and password/token."
- Point out the error display area under the credentials fields
- "After successful login, a **Pull** overlay opens showing streaming docker compose pull output with a 'Skip (start anyway)' button."
- "The pull runs in a background thread, and the frontend polls for updates every 600ms."
- "If all images are cached, the pull step is skipped and containers start immediately."

---

## Scene 13 — Docker Overlay & Start Prompt (9:15 – 9:50)

**Visual**: Docker overlay modal and start prompt.

**Narration**:
> "If Docker isn't installed, the UI shows a 'Download Docker Desktop' link. If it's installed but not running, it shows a Launch Docker button with a polling wait screen. The UI also has an 'Ignore' button to bypass Docker checks entirely."

**Actions**:
- "The overlay also shows a **Start prompt**: 'Start all services?' when Docker is ready but no services are running."
- "Once launched, a waiting screen appears and polls until Docker responds every 500ms."
- "No terminal needed — everything is managed from the browser."

---

## Scene 14 — Wrap Up (9:50 – 10:20)

**Visual**: Back to the full dashboard view.

**Narration**:
> "The Alfresco Control Plane puts everything in one place — service management, logs, files, AMPs, JARs, properties editing, version sync, and theme toggle. No dependencies, no installation, no CLI needed. Just Python and a browser."

**Actions**:
- Slow zoom out / wide view of the full page
- Fade to the http://localhost:9700 URL

---

## Recording Tips

- **Resolution**: 1920×1080 at 60fps
- **Cursor**: Use a visible cursor with a highlight ring
- **Audio**: Clear narration, no background music
- **Pacing**: Pause 2–3 seconds after each action before narrating
- **Zoom**: If needed, zoom browser to 110% for readability
- **Preparation**: Have test `.jar` and `.amp` files ready in a folder before recording
- **Outtakes**: Record each scene separately, then stitch together
- **Tour**: Clear `localStorage` or use the **?** button to restart the guided tour on demand
- **Light/Dark mode**: Toggle with the checkbox in the header to demonstrate theme switching
