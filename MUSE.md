# MUSE.md — Daily NYC techno events update

Run every day at about 9:00 AM America/New_York. Update the events list and artist profiles for the afterhours-nyc app (this repo, branch `main`).

## 1. Date window
Determine today's date in America/New_York (do NOT use UTC). The window is today through 14 days from today, inclusive.

## 2. Find events
Find techno events in New York City (Brooklyn, Queens, Manhattan, Bronx) inside that window. Check ALL of these sources:
- Resident Advisor (https://ra.co/events/us/newyork): query its public GraphQL API at https://ra.co/graphql directly (the normal site is bot-protected). Fall back to a live browser if the API fails.
- DICE (https://dice.fm, New York city): live browser (JS-heavy site).
- Eventbrite (https://www.eventbrite.com/d/ny--new-york/techno-music/): live browser. Results are noisy (concerts, non-music events) — filter carefully.
- Gray Area (https://grayarea.co): NYC electronic music guide, live browser.
Include techno and close styles: hard techno, industrial, EBM, acid, dub techno, minimal, electro. Skip events that are clearly house, pop, hip-hop or other genres.

## 3. Deduplicate
If the same event is listed on multiple sources, include it once and keep the cheapest ticket price; if prices tie or are unknown, prefer the major ticket seller (DICE or RA).

## 4. events.json
Replace the ENTIRE contents of `events.json` with valid JSON in exactly this shape:
```json
{
  "updatedAt": "2026-10-08T09:00:00-04:00",
  "events": [
    {
      "id": "ra-2549344",
      "title": "Event name as listed",
      "lineup": "Artist One, Artist Two, Artist Three",
      "artists": ["Artist One", "Artist Two", "Artist Three"],
      "genres": ["techno", "acid"],
      "venue": "Venue name",
      "area": "Brooklyn",
      "start": "2026-10-09T23:00:00-04:00",
      "source": "dice",
      "price": "$25",
      "priceMin": 25,
      "url": "https://link-to-the-ticket-page"
    }
  ]
}
```
Field rules:
- `updatedAt`: when you made the update, with the New York offset.
- `id`: source plus that site's event number, e.g. `ra-2549344`, `dice-abc123`, `eventbrite-123456`, `grayarea-xyz`. Must be unique.
- `title`: event name as listed.
- `lineup`: artist names in one comma-separated string, exactly as billed. Up to 6 artists; `""` if none listed.
- `artists`: array of individual artist names exactly as billed. Split b2b pairs into separate names. Leave out "TBA", "special guest" and similar placeholders.
- `genres`: array of lowercase genre labels for the event, if the listing shows any. Otherwise `[]`.
- `venue`: venue name.
- `area`: exactly one of `Brooklyn`, `Queens`, `Manhattan`, `Bronx`. Ridgewood and Long Island City venues count as `Queens`.
- `start`: doors time in ISO with New York's offset (-04:00 until Nov 1, 2026, then -05:00).
- `source`: exactly one of `ra`, `dice`, `eventbrite`, `grayarea`.
- `price`: lowest ticket price as text, e.g. `"$25"`. Use `"Free"` if free, `"Sold out"` if sold out, `""` if unknown.
- `priceMin`: lowest ticket price as a number in US dollars, 0 if free, null if unknown.
- `url`: direct ticket or event page link.
- Do NOT include an `"example"` field.
- Valid JSON only (no comments, no trailing commas).

## 5. artists.json
Maintain `artists.json` in the same repo and branch:
```json
{
  "updatedAt": "2026-10-08T09:00:00-04:00",
  "artists": {
    "lord asa": {
      "name": "LORD ASA",
      "genres": ["hardcore techno", "acid"],
      "bio": "Brooklyn-based DJ; hard and fast hardcore techno with trance and acid influences; record label cofounder",
      "followers": { "instagram": 12300, "tiktok": 4100, "soundcloud": 8900, "spotify": 2100 },
      "links": { "instagram": "https://...", "tiktok": "https://...", "soundcloud": "https://...", "spotify": "https://..." },
      "checkedAt": "2026-10-08"
    }
  }
}
```
Rules:
1. Keys are the artist name in lowercase.
2. For every artist in events.json without a profile, find their official Instagram, TikTok, SoundCloud and Spotify accounts. Also use their RA artist page, Bandcamp and press to confirm identity and for the bio.
3. Only use an account when you're confident it's the same artist (matching name plus electronic music context, or linked from their RA page or another official profile). If unsure, leave that platform out. Never guess.
4. `followers`: whole numbers only. For Spotify use followers, not monthly listeners. Leave out platforms you can't verify.
5. `genres`: 1 to 4 lowercase labels describing the music.
6. `bio`: one factual sentence, at most about 35 words, in this style: "Brooklyn-based queer Vietnamese-American DJ (formerly LORD ANNA); hard and fast hardcore techno with trance and acid influences; Wish Records cofounder". When several sources describe the artist, merge the facts into this one summary. No hype words. If nothing reliable is found, leave `bio` out.
7. Keep existing profiles. Refresh follower counts only when `checkedAt` is more than 7 days old. Don't delete profiles of artists who aren't playing anymore.
8. If there are too many new artists to finish in one run, do the ones playing in the next 3 days first and finish the rest on the next run.

## 6. Commit
Save events.json and artists.json together in one commit if possible, with the message `Daily events update YYYY-MM-DD` (NY date). Both files must be valid JSON (no comments, no trailing commas). Read the current sha of each file first (new files need no sha).

## 7. Empty results
If you find zero qualifying events, leave both files unchanged and say so in your report.

## 8. App files
Don't edit index.html, manifest.json, _headers, wrangler.jsonc or .assetsignore. If you think the app needs a change, report it instead.
