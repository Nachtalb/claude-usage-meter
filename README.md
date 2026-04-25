# Claude Usage Meter

A small Chrome / Edge extension that shows how much of your Claude
subscription you've used. The toolbar icon shows the current 5-hour
session percentage as a badge, and clicking it opens a popup with the
full breakdown (current session, weekly all-models, daily routine runs,
extra usage) styled like Claude's own settings page.

## Install (developer mode)

1. Open `chrome://extensions` (or `edge://extensions` for Edge).
2. Toggle **Developer mode** on (top-right).
3. Click **Load unpacked** and select this folder
   (`claude-usage-extension`).
4. Make sure you're signed in to <https://claude.ai>. The extension
   re-uses your normal browser session — there is no token to paste in.

The extension refreshes the data every 5 minutes (and on browser
startup); you can also click the refresh icon in the popup to force a
re-fetch.

## What the badge means

The number on the toolbar icon is the **current 5-hour session
utilization** as reported by `/api/organizations/{id}/usage`.
Color shifts:

- Blue: under 70%
- Amber: 70–89%
- Red: 90% and above
- `!`: not logged in / fetch failed

## Files

- `manifest.json` — MV3 manifest, host permission for `claude.ai`
- `background.js` — service worker; fetches usage, updates the badge
- `popup.html` / `popup.css` / `popup.js` — the popup UI
- `icons/` — extension icons
- `AnthropicSans.woff2` / `AnthropicSerif.woff2` — fonts used in the popup

## Notes / quirks

- Organization id is auto-detected from `/api/organizations` and
  cached in `chrome.storage.local`. If you switch organizations, the
  cached id is cleared automatically on a 401/403.
- "Adjust limit" and "Buy extra usage" simply open
  `https://claude.ai/settings/usage/` in a new tab, since those flows
  live in Claude's own UI.
- The `extra_usage` numbers from the API look like cents
  (e.g. `2003` → `$20.03`); the popup applies a `/100` heuristic
  when the values look like cents.
