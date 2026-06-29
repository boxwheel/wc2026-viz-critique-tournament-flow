# v03 — knockout bracket of 32

- Flywheel node: `ae0067ca-afb2-5ec9-82e9-ef55c4b03a93`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `8fac0c7`
- Plot module:   `plots/v03_bracket_r32.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 4 | **4** | Bracket is canonical for any tournament; agrees. |
| Honesty | 5 | **5** | Empty slots aren't masked with predictions — concur. |
| Clarity | 4 | **3** | Empty R16/QF/SF slots are blank white rectangles with a faint asterisk; the connecting lines are barely visible; the gold "Final" sits orphaned mid-canvas with no SF→Final connector. Reader can't immediately see the tree shape. |
| Aesthetic | 4 | **3** | Right half of the canvas is conspicuously empty; the gold Final box hovers without anchor. |
| Novelty | 3 | **2** | A bracket is a bracket. The confederation strip on the left edge of each team row is mildly novel but doesn't lift it. |

**Mean: 3.4** (vs self 3.8). Author oversold aesthetic and novelty.

## Critique

The viz is honest about what hasn't happened, but the "honesty" creates a
visually empty chart — 75% of the canvas is white pair-boxes. The bracket is
mostly potential, and that potential is invisible. Adding a hint of *who could
fill each slot* (Elo-implied path, seedings, expected confederations) would
turn the empty bracket into a forecast scaffold without committing to fake
results.

## Proposals

### IMPROVE — `v03b_bracket_with_seed_labels`
Fill empty R16 slots with their seed expression ("Winner of R32-Match-3 vs
Winner of R32-Match-4") and dates; draw the SF→Final connector lines so
the tree is whole. Lift the unplayed-slot dim asterisk to a small
confederation-color dot pair (the two possible confs for each slot).
Dataset: `matches_detailed.csv` with bracket-edge inference.
Library: matplotlib.

### PIVOT — `bracket_with_elo_path`
Same tree but encode each team's tile as a fill color proportional to
Elo, and draw faint Elo-weighted ribbons forward from each R32 slot
showing the seed-implied favorite path to the Final. Doesn't predict the
result; shows the implied gravitational structure of the half-draws.
Dataset: `teams.csv` Elo + bracket edges. Library: matplotlib.

### COMBINE with v11 (`4e7860c0` Elo vs pts) — `bracket_with_elo_rail`
Add a right-side rail showing each R32 entrant's Elo (column of dots
aligned to rows). Reveals halves where the seeded favorites are stacked
(making a brutal QF) vs softer halves. Dataset: bracket + Elo.
Library: matplotlib subplot grid.

### EXTEND — `bracket_halves_strength`
Two-panel bar of total Elo per half of the bracket and per quarter,
plus the same for confederation count. Surfaces structural imbalance
("the bottom half has 3 of the top-5 Elo sides"). Dataset: bracket + Elo
+ confederations. Library: matplotlib horizontal bars.
