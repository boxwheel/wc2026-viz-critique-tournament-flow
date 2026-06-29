# v04 — group head-to-head matrices

- Flywheel node: `98e67e2b-3f30-556d-b4aa-9ae497b727bd`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `9b29f0a`
- Plot module:   `plots/v04_group_h2h_matrix.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 5 | **5** | Every group match at correct grain — agrees. |
| Honesty | 5 | **4** | Color encodes *home* GD on a per-cell basis but each fixture only fills one cell (home perspective). A team's row vs column show different fixtures, so a reader skimming a column can misread the cell color as that column-team's perspective. The asymmetry is logically correct but visually misleading without an explicit "home rows / away cols" annotation in the legend strip. |
| Clarity | 4 | **4** | Common GD palette, raw scores overlaid — concur. |
| Aesthetic | 4 | **4** | Calm divergent vlag palette. |
| Novelty | 5 | **5** | Per-group H2H matrix is rare in WC reporting — agrees. |

**Mean: 4.4** (vs self 4.6). Effectively concurring; flagging the home-only
fill convention as a minor honesty risk.

## Critique

This is the strongest of the 10 viz on the meaning/clarity axes. The single
weak spot: a 4×4 fill-half (home only) matrix asks the reader to recognise an
asymmetric encoding. Making the matrix symmetric (both cells of each fixture
filled, with GD negated) doubles ink but removes a real interpretation hazard.
Even keeping the asymmetric form, an explicit "rows = home" callout strip on
each panel would lift the chart from 4 to 5 on honesty.

## Proposals

### IMPROVE — `v04b_h2h_symmetric`
Render both cells of each fixture, GD-sign-inverted across the diagonal,
so any row OR column read consistently. Add a small "home ▲ away ▼"
glyph in each cell corner to record perspective without losing the
score text. Dataset: same `tournament_frame`. Library: matplotlib +
seaborn heatmap.

### COMBINE with v01 (`f29e3669` group small-multiples) — `groups_canonical_page`
A single 12-panel page where each group panel stacks v04 (top) + v01
(bottom) — every game above, every verdict below. Strongest single
artifact for "the group stage". Dataset: shared. Library: matplotlib
`subplot_mosaic`.

### PIVOT — `confederation_h2h_aggregate`
Roll up across groups: average home-team GD when conf A plays conf B
(6×6 matrix). Surfaces structural patterns ("UEFA wins by 0.7 GD over
AFC on average") in one image. Dataset: `tournament_frame` joined to
`teams.csv` for confs. Library: seaborn heatmap.

### EXTEND — `group_xg_h2h_matrix`
Same 4×4 per-group form but cell color = home xG − away xG (using
`team_stats.csv`), cell text = "xG_home – xG_away". A direct comparison
against v04's scoreline GD will reveal teams who "lost the xG but won
the match" group-stage-wide. Dataset: `team_stats.csv`. Library:
seaborn heatmap.
