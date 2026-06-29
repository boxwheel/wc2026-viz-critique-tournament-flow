# v06 — upset map (Elo gap × goal difference)

- Flywheel node: `7a22b8a3-c8a0-53fd-8292-2ba9b00da7af`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `f10c3eb`
- Plot module:   `plots/v06_upset_map.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 5 | **5** | Defines "upset" for the dataset and reports the count (6/72) — agrees. |
| Honesty | 5 | **4** | The "80 Elo ≈ 1 goal" diagonal is an unsupported assertion drawn as if a model; either fit it from match data or remove. Otherwise plot is data-faithful. |
| Clarity | 4 | **3** | All five top-left outright upsets cluster in a vertical strip at -200<Elo gap<-100; the labels stack and overlap with leader lines crossing each other. Reader can't easily attribute a label to its dot. |
| Aesthetic | 4 | **4** | Calm field with gold accents. |
| Novelty | 4 | **4** | Framing is uncommon in WC reporting — agrees. |

**Mean: 4.0** (vs self 4.4). Disagree on clarity due to overlap; honesty
clipped due to undocumented diagonal.

## Critique

Strong concept (a single 2D space that *defines* "upset"), weakened by two
specific flaws: the labels for the most-interesting points pile on top of each
other, and a heuristic line is drawn without provenance. Both are cheap fixes
that take this viz from 4 → 4.6.

## Proposals

### IMPROVE — `v06b_upset_map_v2`
Use `adjustText` to push the 6 upset labels apart with leader lines that
don't cross; replace the "80 Elo ≈ 1 goal" diagonal with a least-squares
fit of GD-vs-Elo-gap over all 72 matches plus a ±1σ band, with the
equation in the corner. Dataset: same `tournament_frame` + Elo.
Library: matplotlib + `adjustText` + numpy polyfit.

### PIVOT — `top_surprises_bar`
Rank the 10 most surprising results by (expected GD − actual GD)
deviation, render as a horizontal bar chart with the match text. Same
answer to "what surprised", much higher data:ink and easier to scan.
Dataset: same. Library: matplotlib h-bars.

### COMBINE with v11 (`4e7860c0` Elo vs pts) — `elo_outliers_diptych`
Two-panel: v06 match-level (upsets) on the left, v11 team-level
(over/under-performers) on the right, with cross-highlighting (Mexico's
three wins highlighted in v06 when Mexico is the over-performer in
v11). Dataset: shared. Library: matplotlib gridspec.

### EXTEND — `pre_tournament_elo_gap_dist`
Histogram of all 72 group-match Elo gaps showing how unbalanced the
draw was (lots of >300-Elo mismatches means few opportunities for
upsets in the first place — context for the 6/72 count). Dataset: same.
Library: matplotlib hist + reference line at conf-average gap.
