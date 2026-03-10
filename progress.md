# Progress

## Status Key
`[ ]` todo Â· `[x]` done Â· `[~]` in progress Â· `[!]` blocked

---

## Setup
- [x] Supabase project created, schema + RLS applied
- [x] `.env` file created with SUPABASE_URL + SUPABASE_ANON_KEY
- [x] `.gitignore` created (ensure `.env` is listed in it)
- [ ] Vercel account - defer to Week 2

## Week 1 - Extension Core
- [ ] `manifest.json` with permissions + externally_connectable
- [ ] `popup.html` + `popup.js` - list tab groups, share button
- [ ] `service_worker.js` - POST to Supabase, return share URL
- [x] Copy link to clipboard on success

## Week 2 - Receiver Page
- [ ] `supabase/schema.sql` - table + RLS
- [ ] `/g/[id]` page - fetch + render group card
- [x] Fallback "Open all tabs" (no extension)

## Week 3 - Full Loop
- [ ] `service_worker.js` - handle incoming message, recreate group
- [ ] Receiver page - detect extension, show correct CTA

## Week 4 - Polish
- [ ] Favicons in tab list
- [ ] Error states (expired ID, fetch fail, no tabs)
- [ ] Install prompt if extension missing
- [ ] Web Store submission prep

---

## Decisions Log
| Date | Decision |
|---|---|
| - | Groups open in current window (not new) |
| - | Ungrouped tabs ignored by default |
| - | No expiry in v1 |
| - | Vercel setup deferred to Week 2 |

