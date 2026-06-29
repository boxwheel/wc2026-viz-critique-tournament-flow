# v01 — group-stage small-multiples (diverging bars)

- Flywheel node: `f29e3669-e7d0-58c7-9560-8afd7e9aa9ee`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `252ec64`
- Plot module:   `plots/v01_group_standings_small_multiples.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 4 | **4** | Whole group stage verdict in one read; agrees. |
| Honesty | 5 | **5** | Shared bar scale, no truncated axes — concur. |
| Clarity | 4 | **3** | In-bar score numbers and gold rank dots are tiny at thumbnail size; "12 GA / 12 GF" baseline marks are read-only-if-you-know. The reader must hunt the gold dot. |
| Aesthetic | 4 | **4** | Calm grid, restrained palette. |
| Novelty | 4 | **3** | Diverging-bar standings has been done; "small-multiples of standings tables" is bread-and-butter. |

**Mean: 3.8** (vs self 4.2). Disagreement: clarity & novelty oversold.

## Critique

The diverging-bar encoding is sound but its specific instantiation buries the
advancement signal in micro-elements (a small gold dot per row). A reader's eye
should land on "who advanced" in <1 second; right now they have to spot 4
shades of dot at 8px against a busy background. Numbers in the bars are also
sub-readable at the rendered size — they're decoration, not data.

## Proposals

### IMPROVE — `v01b_group_small_multiples_v2`
Fix the readability flaws while keeping the form: shade the top-two row's
background a faint green/gold band so "advanced" reads pre-attentively; drop
the in-bar score text in favor of a single large GD number on the right
margin; remove redundant per-panel "GA/GF" axis labels (state once in the
caption). Dataset: same `group_standings` frame. Library: matplotlib +
`highlight_text` for inline color emphasis.

### COMBINE with v04 (`98e67e2b` group H2H matrices) — `groups_canonical_page`
A single 12-panel grid where each panel is the v04 4×4 score-matrix
*above* the v01 diverging bars — top half = "every game", bottom half =
"the verdict". One artifact replaces two and pays off both forms.
Dataset: `tournament_frame` + `group_standings`. Library: matplotlib
`subplot_mosaic` per group panel.

### PIVOT — `group_bumpchart`
Slope/bump chart of running points after matchday 1 → 2 → 3, one line per
team, faceted by group (12 panels). Reveals the order in which advancers
locked it up vs late twists (e.g., Germany losing to Ecuador first then
recovering). Dataset: derive cumulative pts per team per matchday from
`matches_detailed.csv`. Library: matplotlib lines + dot markers per
matchday.

### EXTEND — `group_panel_with_goal_minutes`
Append a thin 0–90 minute timeline strip beneath each group panel
showing the 12 goals scored in that group as colored dots (color =
scoring team), exposing the temporal texture per group. Dataset:
`match_events.csv` filtered to goals, joined to group. Library:
matplotlib strip plot.
