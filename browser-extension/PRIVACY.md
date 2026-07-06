# Privacy Policy — Obsidian Task Manager (Browser Extension)

**Effective date:** 2026-07-06

This browser extension creates tasks in the user's own **Obsidian Task Manager** plugin, which
runs locally inside Obsidian on the user's computer. This policy explains exactly what the
extension accesses, why, and where it goes.

**Summary:** The extension does not have any servers of its own. It sends data only to the
user's own Obsidian plugin over the local loopback address on the user's machine. No data is
collected by the developer, transmitted to any remote or third-party server, sold, or shared.

## What the extension accesses, and why

The extension only accesses data in direct response to an action the user takes (clicking the
toolbar button, using a right-click menu item, or clicking the "OTM" button inside an open
email). Specifically:

- **Current tab title and URL** — read when the user chooses to turn the current page into a
  task, so the page title can pre-fill the task and the page URL can be saved as the task's
  source link.
- **Selected text** — read when the user chooses "Create Obsidian task from selection," so the
  highlighted text becomes the task.
- **Open email subject and sender (Gmail only, on mail.google.com)** — read when the user
  clicks the extension's "OTM" button inside an opened email, so the subject becomes the task
  title, the sender is noted, and a link back to that email is saved as the task's source.
- **Extension settings** — the connection details (host, port, and an optional API token) and
  the user's chosen defaults (default group, priority, Kanban board, and whether to open new
  tasks in Obsidian).

The extension does **not** read your inbox, browsing history, keystrokes, passwords, or any
page you have not explicitly acted on.

## How data is used and where it goes

- Task data (title, notes, tags, source link, etc.) is sent **only** to the user's own Obsidian
  Task Manager plugin via its local HTTP API at `http://127.0.0.1` / `http://localhost` on a
  port the user configures. This request never leaves the user's computer.
- Extension settings are stored locally on the user's device using the browser's
  `chrome.storage.local` API. They are not transmitted anywhere.
- The extension contains **no analytics, tracking, advertising, or third-party code**, and
  makes **no requests to any remote or external server**.

## Data sharing and sale

None. The developer does not receive, collect, store, sell, or share any user data. Because all
communication is with software running on the user's own machine, no personal data is
transmitted to the developer or to any third party.

## Data retention and deletion

- Tasks created by the extension are stored by the user's Obsidian plugin as files in the
  user's own Obsidian vault; the user controls and can delete them at any time.
- Extension settings remain in the browser's local storage until the user changes them or
  removes the extension. Uninstalling the extension removes its stored settings.

## Permissions

- `storage` — save the user's settings locally.
- `activeTab` — read the current tab's title/URL when the user creates a task from a page.
- `contextMenus` — provide the "Create task from selection" and "Add this page" right-click
  items.
- `notifications` — show a brief success/error message after creating a task.
- Host access to `http://127.0.0.1/*` and `http://localhost/*` — talk to the user's local
  Obsidian plugin.
- Host access to `https://mail.google.com/*` — add the "create task" button inside an opened
  Gmail email.

## Changes to this policy

If this policy changes, the updated version will be published at the same location with a new
effective date.

## Contact

Questions about this policy: **antoneheyward@gmail.com**
