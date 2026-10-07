# Daily update instructions for Muse

Paste the goal below into Muse. Replace `YOUR-GITHUB-NAME` with your GitHub username.

---

Every day at 9am New York time, update the NYC techno events list for my app.

1. Find techno events in New York City (Brooklyn, Queens, Manhattan, Bronx) happening from today through 14 days from now. Check Resident Advisor (ra.co/events/us/newyork) and DICE (dice.fm, New York). Include techno and close styles: hard techno, industrial, EBM, acid, dub techno, minimal, electro. Skip events that are clearly house, pop, hip-hop or other genres.
2. If the same event is listed on both RA and DICE, include it once and prefer the DICE ticket link.
3. Replace the entire contents of the file `events.json` (create it if it doesn't exist) in my GitHub repo `YOUR-GITHUB-NAME/afterhours-nyc` (branch `main`) with valid JSON in exactly this format:

```json
{
  "updatedAt": "2026-10-08T09:00:00-04:00",
  "events": [
    {
      "id": "ra-2549344",
      "title": "Event name as listed",
      "lineup": "Artist One, Artist Two, Artist Three",
      "venue": "Venue name",
      "area": "Brooklyn",
      "start": "2026-10-09T23:00:00-04:00",
      "source": "dice",
      "price": "$25",
      "url": "https://link-to-the-ticket-page"
    }
  ]
}
```

Rules for each field:
- `updatedAt`: the time you made this update, with the New York offset.
- `id`: the source plus that site's event number, like `ra-2549344` or `dice-abc123`. Must be unique.
- `area`: exactly one of `Brooklyn`, `Queens`, `Manhattan`, `Bronx`. Ridgewood and Long Island City venues count as `Queens`.
- `start`: doors time in ISO format with New York's offset (`-04:00` until Nov 1, 2026, then `-05:00`).
- `source`: lowercase, `ra` or `dice`.
- `price`: the lowest ticket price shown, like `$25`. Use `Sold out` if sold out, or `""` if unknown.
- `url`: the direct ticket or event page link.
- `lineup`: up to 6 artists, comma-separated. `""` if not listed.
- Do not include the `"example"` field.
- The file must be valid JSON (no comments, no trailing commas). If you can't find any events, keep the previous file unchanged.

Commit message: `Daily events update YYYY-MM-DD`.

---

After the first run, open the app and check that "Updated" at the top shows today's date.
