# Privacy Policy — Gameplay Detector

**Last updated:** March 11, 2026

This privacy policy describes how the **Gameplay Detector** Chrome extension (“the Extension”) handles information. The Extension is designed to detect gameplay on the active browser tab and optionally block it or send notifications. We do not sell your data.

---

## 1. What the Extension Does

- Analyzes **only the tab you are currently viewing** to detect gameplay using:
  - URL rules (built-in and custom allow/block lists)
  - Page structure (e.g., canvas, video elements)
  - An on-device machine-learning model (runs locally in your browser; no screenshots or page content are sent to our servers for analysis)
- Can redirect the tab to a “blocked” page after a short countdown when gameplay is confirmed
- Can send a notification to **Google Chat** with a summary (URL, time, confidence) and optional buttons to add/remove from whitelist or keep blocking
- Can use an **optional local database** (a server you run at `http://127.0.0.1:8080`) to store “game” or “not game” per URL

---

## 2. Data Stored on Your Device

All of the following is stored **only in your browser** using Chrome’s local extension storage (`chrome.storage.local`). We do not have access to it.

| Data | Purpose |
|------|--------|
| **Settings** | Whether detection is on/off and whether the optional database is enabled |
| **Whitelist / blocklist** | Origins (e.g. `https://example.com`) you added via the extension or via Google Chat buttons (Add to whitelist, Remove from whitelist, Keep blocking) |
| **Recent decision state** | A temporary “report already sent” marker (tab id and timestamp) to avoid duplicate notifications; cleared when you close the tab or after a short cooldown |
| **Activity log** (optional) | A small list of recent detections: URL, page title, time, and confidence scores only (no screenshots or images). Used only for the blocked page and popup display. |
| **Decisions cache** | Cached copies of decisions (e.g. whitelist/block) received from the decision service so the extension can apply them without re-polling immediately |

None of this data is sent to our servers for storage. You can clear it by removing the extension or clearing the extension’s storage.

---

## 3. Data Sent to Third-Party Services

### 3.1 Google Chat (optional notifications)

When the Extension sends a notification after a detection is confirmed, it sends a **one-way message** to a Google Chat space (via a webhook URL configured in the Extension). That message may include:

- **Blocked URL** (the page where gameplay was detected)
- **Time** of the detection
- **Detection confidence** (numeric scores for URL, DOM, and AI model)
- **Your display name** (from Chrome’s signed-in account, if you have granted the “identity” permission; otherwise “Chrome user”)

Google’s privacy policy applies to data processed by Google Chat. We do not control or store that data on our own servers.

### 3.2 Decision service (Cloudflare Worker)

When you click a button in the Google Chat card (e.g. “Add to whitelist”, “Remove from whitelist”, “Keep blocking”), your browser opens a link to a **decision API** (e.g. a Cloudflare Worker). That request sends:

- An **event identifier** (derived from detection time, not your identity)
- The **action** you chose (whitelist / unwhitelist / block)

The Extension also **polls** that service (with the same event id) to read back the stored action so it can update the local whitelist/blocklist and send a confirmation message to Google Chat. The worker may store the event id and chosen action temporarily (e.g. with a TTL). We do not use this data to track you across sites or over time beyond fulfilling the single decision.

### 3.3 Local database (optional, your own server)

If you enable the “database” feature and run a local server (e.g. at `http://127.0.0.1:8080`), the Extension sends **only**:

- The **URL** of the page you are viewing
- A **status** (e.g. “game” / “not game” / “never seen”) for lookup and, when applicable, for saving your classification

That data stays on your machine. We do not have access to your local server.

---

## 4. What We Do Not Do

- We do **not** collect or store your browsing history, screenshots, or page content on our servers.
- We do **not** use remote code: all extension logic and the detection model run from the installed extension package.
- We do **not** sell or share your data for advertising or other commercial purposes.
- The Extension does **not** analyze or capture tabs you are not currently viewing.

---

## 5. Chrome Permissions — Why We Use Them

| Permission | Use |
|------------|-----|
| **Tabs** | To know which tab is active and to redirect that tab to the blocked page when you have chosen to block. |
| **Active tab** | To capture the visible area of the active tab only when detection is enabled; used only for local analysis. |
| **Storage** | To save your settings, whitelist/blocklist, and the other local data described above. |
| **Downloads** | Only if you enable optional “debug screenshots” to save sample images locally; otherwise unused. |
| **Offscreen** | To run the on-device AI model (TensorFlow.js) in a hidden page; all processing is local. |
| **Scripting** | To inject the extension’s own content script (e.g. countdown) into the active tab when needed. |
| **Identity / Identity email** | To show your name in Google Chat notifications; detection works without this (we then use “Chrome user”). |
| **Host access (<all_urls>)** | To analyze the active tab on any site, to call the decision API when you use Chat buttons, and to call your optional local database. |

---

## 6. Changes to This Policy

We may update this privacy policy from time to time. The “Last updated” date at the top will be revised when we do. Continued use of the Extension after changes means you accept the updated policy.

---

## 7. Contact

If you have questions about this privacy policy or the Extension’s data practices, please contact us at xiaofengsimon.xu@mail.utoronto.ca.
