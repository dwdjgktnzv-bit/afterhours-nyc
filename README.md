# Afterhours NYC

A phone-first list of NYC techno events, filterable by night and borough.

- `index.html` — the app. It reads `events.json` and shows it.
- `events.json` — the event list. Muse replaces this file every day (see `MUSE.md`).
- `manifest.json` — lets the app be added to a phone's home screen.
- `_headers` — tells Cloudflare not to cache `events.json`, so daily updates show up right away.
- `MUSE.md` — the daily instruction to give Muse.

Hosted on Cloudflare Pages, connected to this repo: every change to `events.json` republishes the site automatically. No build step.
