# Baseball tools

Two single-file web tools — open either in any browser, no server or dependencies needed:

- [`savant-link-generator.html`](savant-link-generator.html) — builds Baseball Savant
  Statcast search links from a saved template.
- [`build.html`](build.html) — upload a DraftKings DKSalaries CSV and view the slate's
  player pool as a sortable, filterable table.
- [`merge.html`](merge.html) — merge a DKSalaries CSV with Savant detail CSVs into
  batter and pitcher tables: salary and FPPG next to aggregated Statcast stats.

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

`build.html` turns an uploaded DKSalaries CSV (the bulk-entry template from any
contest's Export to CSV) into a sortable, filterable player table: name, position,
roster slot, salary, team, game, start time, FPPG, with search plus position and game
filters. The parser locates the header row anywhere in the sheet, since DK pads the
player table to the right of the roster grid beneath an instructions block. Draftables
JSON blobs and pasted text work too.

## Merge

`merge.html` joins a DKSalaries CSV with Baseball Savant `statcast_search` CSVs,
matched by normalized name (handles `Last, First` vs `First Last`, accents, and
Jr./II suffixes), into two always-visible tables — Batters and Pitchers — each with
its own search, only-matched filter, and sortable columns.

Detail CSVs (player & event searches) are not averaged away: because those searches
already filter to hard-hit balls, each player row shows counts and tiers — events,
HR, hits, 105+/110+ EV counts, max EV, 400ft+ balls, max distance — plus an EV dot
strip (every batted ball positioned by exit velo; HR filled accent, other hits filled
blue, outs hollow), and clicking a row opens the full event log (date, result, EV,
LA, distance). Grouped-stats CSVs (group by player, e.g. the pitcher stat search) are
used as-is with their numeric columns carried into the table dynamically. Parameter names and encoding (`hfGT`, `hfSea`, `hfTeam`,
`hfFlag`, `metric_1`, …) match the original template link byte-for-byte, so the
search behaves identically on baseballsavant.mlb.com.
