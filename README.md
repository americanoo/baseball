# Baseball Savant link generator

A single-file web tool ([`savant-link-generator.html`](savant-link-generator.html)) that builds
Baseball Savant Statcast search links from a saved template. Open the file in any browser —
no server or dependencies needed.

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
same query, returning pitch-level detail rows (capped by Savant at 25,000 rows). Parameter names and encoding (`hfGT`, `hfSea`, `hfTeam`,
`hfFlag`, `metric_1`, …) match the original template link byte-for-byte, so the
search behaves identically on baseballsavant.mlb.com.
