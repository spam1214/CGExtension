# Project Structure

```
tabshare/
├── extension/
│   ├── manifest.json
│   ├── popup/
│   │   ├── popup.html
│   │   └── popup.js
│   └── background/
│       └── service_worker.js
│
├── web/                        # Vercel receiver page
│   ├── index.html              # landing / marketing
│   └── g/
│       └── [id].html           # dynamic group page (or JS router)
│
└── supabase/
    └── schema.sql              # table definition + RLS policies
```

## Key Boundaries
- `extension/` has no knowledge of `web/` except the share URL format
- `web/` talks to extension only via `chrome.runtime.sendMessage`
- `supabase/` is the only place DB schema is defined