# PRD: Tab Group Sharer — Chrome Extension

## Problem

Chrome tab groups are profile-local. Sharing requires manual link copying or screenshots — slow, error-prone, and non-scalable for teams.

## Goal

A Chrome extension that packages a tab group into a shareable link. Receivers open the link and recreate the group in one click.

## Target Users

- Students sharing research tabs
- Developers sharing docs/debugging setups
- Teams sharing project reading lists
- Trip planners sharing booking tabs

---

## Tech Stack (Free)

| Layer | Choice |
|---|---|
| Extension | Chrome MV3 (Vanilla JS) |
| Backend | Supabase (free tier) |
| Hosting | Vercel (free tier) |
| Short URL | nanoid stored as Supabase row ID |

---

## Core Features

**Share Side (Sender)**
1. User clicks extension icon → sees list of open tab groups (ungrouped tabs are ignored)
2. Selects a group → clicks "Share"
3. Extension reads group name, color, and all tab URLs via `chrome.tabGroups` + `chrome.tabs` APIs
4. POSTs payload to Supabase → gets back a short ID
5. Extension shows a copyable link: `https://tabshare.vercel.app/g/{id}`

**Receive Side (Receiver)**
1. Receiver opens the link in browser
2. Web page shows group name, color, list of URLs
3. If extension is installed: "Open Tab Group" button → calls `chrome.runtime.sendMessage` → extension recreates the group in the current window
4. If extension not installed: shows install prompt + fallback "Open all tabs" button (no grouping)

---

## Data Model

```sql
id          text PRIMARY KEY   -- nanoid, 8 chars
name        text
color       text               -- chrome color token (e.g. "blue")
tabs        jsonb              -- [{url, title, favicon}]
created_at  timestamptz DEFAULT now()
```

---

## Extension Architecture

```
extension/
├── manifest.json          # MV3, permissions: tabGroups, tabs
├── popup/
│   ├── popup.html
│   └── popup.js           # list groups, share button, copy link
└── background/
    └── service_worker.js  # receive message → create tab group
```

**Key permissions:** `tabGroups`, `tabs`, `scripting`, `storage`

`manifest.json` must include `externally_connectable` for `tabshare.vercel.app` so the receiver page can message the extension across origins.

**Message flow (receiver page → extension):**
```
page: chrome.runtime.sendMessage(EXT_ID, { action: "openGroup", data })
↓
service_worker.js: creates tabs + groups them via chrome.tabGroups.update()
```

---

## Receiver Web Page (Vercel)

Single-page app (`/g/[id]`):
1. Fetch group data from Supabase by ID
2. Render group card (name, color badge, URL list with favicons + titles)
3. Detect extension via `chrome.runtime` ping
4. Show "Open Tab Group" or "Install Extension" CTA

---

## Supabase Setup

- Row Level Security: anon insert allowed, select by ID allowed
- Anon key is safe — rows are write-once, no sensitive data

---

## Constraints & Risks

| Risk | Mitigation |
|---|---|
| Receiver has no extension | Fallback "open all tabs" + install prompt |
| Auth tokens in URLs shared accidentally | Warn user: "Do not share tabs with sensitive sessions" |
| Supabase free tier (500MB, 2GB egress) | Each row ~2KB; supports ~250K groups free |

---

## Milestones

| Week | Deliverable |
|---|---|
| 1 | Extension reads tab groups, POSTs to Supabase, copies link |
| 2 | Receiver page (Vercel) renders group + opens tabs fallback |
| 3 | Service worker recreates full tab group on message |
| 4 | Polish: favicons, error states, install prompt, Web Store submission |

---

## Success Metrics

- Share flow completes in < 5 seconds
- Receiver recreates group in 1 click
- Works on Chrome 114+ (tabGroups API stable)