# Baseball tools

Two single-file web tools — open either in any browser, no server or dependencies needed:

- [`savant-link-generator.html`](savant-link-generator.html) — builds Baseball Savant
  Statcast search links from a saved template.
- [`build.html`](build.html) — lists that day's DraftKings MLB slates and downloads the
  "build a lineup" player CSV (name, ID, position, salary, game info) for each.

## Savant link generator

The form starts pre-filled with the template's values:

| Variable | Template default |
| --- | --- |
| Season / game type | 2026, regular season |
| Player type | Batter |
| Games on or after | 2026-08-02 |
| Teams | 20 of 30 (a "Template 20" button restores this set) |
| Batted-ball filter | Exit velocity ≥ 95 mph, bunts excluded |
| Results | Grouped by player & event, sorted by projected distance (desc) |

The generated link updates live as you change any field, with Copy,
Open-in-Savant, and Download CSV buttons. The CSV button hits Savant's own
export endpoint (`/statcast_search/csv` with `all=true&type=details`) using the
same query, returning pitch-level detail rows (capped by Savant at 25,000 rows).

## DK build

`build.html` discovers slates from DraftKings' lobby feed
(`draftkings.com/lobby/getcontests?sport=MLB`): it tries fetching directly, and when the
browser blocks the cross-site read, a one-click manual route opens the feed so you can
paste the JSON in. Slates for the selected day (default: today, US Eastern) are listed
with start time, slate label, contest type, and game count. Each row's Download CSV
button hits DraftKings' player-export endpoint
(`draftkings.com/lineup/getavailableplayerscsv?contestTypeId=…&draftGroupId=…`) — the
same CSV the Build Lineup / bulk-entry flow uses — and Download-all grabs every slate
shown, staggered so the browser saves each file. Parameter names and encoding (`hfGT`, `hfSea`, `hfTeam`,
`hfFlag`, `metric_1`, …) match the original template link byte-for-byte, so the
search behaves identically on baseballsavant.mlb.com.
