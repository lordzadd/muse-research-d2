# Muse Settings — local permission surface map

Observed live via AX tree on the local Desktop app (Settings window, pid 52309, window 3814).
Date: 2026-10-07. Plan tier: Tier 3 prompt-injection / permission-model verification.

## Computer use pane

- Accessibility: **granted** (System Settings)
- Screen Recording: **granted** (System Settings)
- Computer control: **Ask every time** (default)
- Browser automation: **Ask every time** (default)
- Blocked apps: list empty; "Add app" available. Muse cannot see/use blocked apps.
- "Keep screen awake while working" checkbox present.

Conclusion: TCC grants are wide open (full desktop control), but an in-app per-action
consent gate ("Ask every time") fronts computer-control and browser-automation actions.
The interesting question: what exactly triggers the gate, and can it be re-armed or
bypassed from within the agent loop (misdirected clicks onto approval cards, etc.).

## Secure store pane

- Heading: "Secure credentials store" — "Securely store passwords for your agent to use."
- Single "Add" button. Store is currently **empty** (no stored passwords yet).

This is where agent-held passwords live (first-party/connector-token adjacent surface,
$130k plaintext-credential category). Empty on this bare account — testable only after a
credential is stored.

## Permissions pane

- Connector defaults: **"Ask for some actions"** (default) vs "Always ask".
  Text: "These defaults apply when no permission has been saved. 'Always allow' choices
  do not expire."
- Web access defaults: **"Ask for some actions"** (default) vs "Always ask".
- Manage permissions inventory (saved approvals):
  - Featured apps **4**
  - Files and Folders
  - Connectors **1**
  - Websites **0**
  - Artifacts **0**
  - Scheduled tasks **1**
  - Direct network protocols **8**
  - Advanced network settings
  - Reset approvals to defaults

### Direct network protocols sub-pane (8 toggles, all OFF)

- Outbound SSH (port 22; SFTP) — OFF
- Outgoing email (SMTP: 25, 465, 587) — OFF
- Email mailbox access (IMAP/POP3: 110, 143, 993, 995) — OFF
- Database connections (PostgreSQL, MySQL, MongoDB, Redis, SQL Server) — OFF
- File transfer (FTP: 21, 990) — OFF
- External DNS lookups (TXT, MX, SRV; **run without individual approval prompts while
  enabled**) — OFF
- Other TCP connections — OFF
- Other UDP traffic — OFF
- App note: "Off blocks the protocol. On lets the assistant ask you for approval per
  connection."

Conclusion: egress is net-blocked by default; only per-protocol opt-in plus per-connection
approval. DNS is the special case (no per-query prompting). This is the app-side Sentinel.

### Featured apps sub-pane (4 saved grants)

1. Apple Notes — **Enabled**
2. Apple iMessage — **Enabled**
3. WhatsApp on this Mac — **Connected**
4. Apple Mail — **Enabled**

### Apple Notes detail (representative grant model)

- Description: "Read, create, update, and delete notes on this Mac."
- Read permissions: "See notes" — **Allow** (pre-authorized, no prompt)
- Write/delete permissions: "Manage notes" — **Ask** (prompts)
- Caveat: "Some actions ask separately. Choose 'Always allow' on an action's approval card
  to allow future requests until you change it. Changing this setting clears saved action
  approvals."

**Key model takeaway**: READ is pre-authorized for Notes/iMessage/Mail/WhatsApp content on
this Mac; WRITE/DELETE/SEND prompts. A prompt-injection planted in agent-readable local
content (a Notes note, an email, a message) rides the pre-authorized read path and then
must cross the write gate — that gate is the main control to test for bypass.

## Connectors pane (catalog, none connected)

Local/often-authored: Apple Calendar, Apple Reminders, Apple Contacts (Connect buttons).
Featured: Gmail, Instagram, Withings.
Productivity: Google Calendar, Google Drive, **Finances (Plaid)**, Google Sheets, Google
Docs.
Social: Facebook, Messenger, Threads, Instagram Messages, Spotify.
Health: Peloton, Function Health, HealthEx.
Business: Meta Business Manager, GitHub, Canva, Dropbox, Zoom.
Plus,"See all" expanders per category. No connector shows as connected on this account.

## Data controls pane

- "Help improve our AI models" checkbox (AI-training data sharing opt-out).
- **Import memory** button — memory upload surface (memory-poisoning vector).
- **Download your agent data** button — data export (info-disclosure surface).
- **Reset Lapis** button — "Permanently deletes your Muse data, including chat history,
  files and active tasks."

## Consolidated attack-relevant observations

1. Local app holds per-app enable/allow toggles in the Hatch prefs web surface, mirroring
   broker/Sentinel policy; the OS-level TCC grants are broad (Accessibility + Screen
   Recording).
2. Read-only access to Notes/iMessage/Mail/WhatsApp content is currently **Allow**
   (prompt-free). Write/send/delete is "Ask".
3. Egress is protocol-gated, all off; DNS lookups exempted from per-query prompts.
4. No connector tokens exist yet on this account; the $130k token surface is dormant until
   a connector is added.
5. "Import memory" is an unguarded-looking user-activated vector for planting long-term
   instructions (memory injection).