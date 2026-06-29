# v10 — venues map (capacity, location, goals)

- Flywheel node: `4f4215b3-3151-5aa6-b5e7-6b5819636fe8`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `e0cd93f`
- Plot module:   `plots/v10_venues_map.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 4 | **3** | Ties geography + capacity + goals — but the country band shortcut undermines the geography. |
| Honesty | 4 | **2** | Three problems: (a) "Canada/USA/Mexico" are rendered as *horizontal latitude bands* rather than country shapes — Vancouver is in the same band as Toronto despite being on different US-border behavior; (b) several capacity labels are truncated ("0k", "1k", "k" instead of "70k", "61k", "80k") — confirmed visually; (c) the node's own summary names "MetLife and AT&T (Arlington) doing the heaviest goalscoring" while the content body names "Earth City/St Louis (24g), Atlanta (18g)" — internal contradiction. |
| Clarity | 3 | **2** | US east-coast cluster (Atlanta / Foxborough / Earth City / Toronto / Miami) is unreadably crowded; size legend at bottom-right overlaps the goals colorbar at bottom-left visually. |
| Aesthetic | 4 | **3** | Soft country bands look pleasant but mislead; everything else is fine. |
| Novelty | 4 | **3** | Capacity-and-goals dual encoding on a venue map isn't novel for sports analytics. |

**Mean: 2.6** (vs self 3.8). This is the weakest of the 10. **Strong
disagreement on honesty** — the chart fails the rubric's "honesty" axis.

## Critique

Three independent flaws compound into a viz that doesn't deliver. The country
bands are a graphic shortcut that *replaces* geography with a stripe of latitude
bins; the label truncation visibly mis-states stadium capacities ("0k" for a
70k venue is comically wrong); and the headline summary disagrees with the
content body about which venues scored the most. The chart needs a re-render,
not a tweak.

## Proposals

### IMPROVE — `v10b_venues_map_fixed` *(highest priority)*
- Replace horizontal country bands with a real North-American basemap
  (`geopandas` + Natural Earth lo-res countries; offline files in
  `/data/naturalearth/`); or use `cartopy` with a `PlateCarree`
  projection.
- Fix label-truncation bug: render capacity as `f"{int(cap_k)}k"` not
  the substring-slicing that produced "0k".
- Reconcile summary vs body: re-derive top-goal venues from
  `matches_detailed.csv` joined to `venues.csv` and write **one**
  ranking in both the node summary and the body.
- Use `adjustText` to push east-coast labels apart with leader lines.
Library: geopandas + matplotlib + `adjustText`. Critical: this chart
needs to pass honesty before it ships again.

### PIVOT — `venues_dot_strip`
Drop the map and render three vertical strips (USA / MEX / CAN), each
strip a vertically sorted list of venues with two encodings per row
(capacity bar + goals heat-cell). Removes the geographic crowding
entirely. Dataset: `venues.csv` + `matches_detailed.csv`.
Library: matplotlib.

### COMBINE with v05 (`98ada01f` goals timeline) — `venue_minute_facets`
For each venue, render a small minute-of-goals histogram (the v05
marginal distribution, but per venue). Tests whether the late-goal
phenomenon is venue-invariant or driven by certain stadiums (altitude,
weather). Dataset: `match_events.csv` joined to `venues.csv`.
Library: matplotlib small-multiples.

### EXTEND — `team_travel_paths`
Travel distance per team across their 3 group matches as path-lines on
a real basemap, total km in a side bar, colored by team. Surfaces
schedule-difficulty asymmetry (does Group I travel more than Group A?).
Dataset: team → match → venue → coords. Library: cartopy + matplotlib.
