# Agent Instructions: Tab Group Sharer

## What You're Building
Chrome MV3 extension + Vercel receiver page + Supabase backend.
Users share tab groups via a link. Receivers recreate the group in one click.

## Source of Truth
- `prd.md` — full spec, data model, milestones
- `structure.md` — exact file layout to follow
- `progress.md` — check before starting; update after each task

## Rules
- Always check `progress.md` first
- Never create files not in `structure.md` without flagging it
- Use vanilla JS only — no React, no build tools
- All Supabase calls use the anon key from env; never hardcode secrets
- `externally_connectable` in manifest must include the Vercel domain
- Ungrouped tabs are always ignored when listing groups
- Tab groups open in the **current window**, not a new one

## Env Vars (Vercel + extension storage)
```
SUPABASE_URL=
SUPABASE_ANON_KEY=
EXTENSION_ID=        # set after Chrome Web Store publish
```

## Stack
| Layer | Tool |
|---|---|
| Extension | Chrome MV3, Vanilla JS |
| DB + API | Supabase (free) |
| Receiver page | Vercel (free) |
| ID generation | nanoid (8 chars) |