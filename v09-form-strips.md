# v09 — 48-team form strips (W/D/L)

- Flywheel node: `686de29a-1c94-563c-a79c-ddc5d22644a0`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `fe1a266`
- Plot module:   `plots/v09_team_form_strips.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 4 | **4** | Team-level arc across three matchdays — agrees. |
| Honesty | 5 | **5** | Raw outcomes, real opponent names, no smoothing — agrees. |
| Clarity | 4 | **4** | Score text inside cells is small but legible at 200dpi; 3-letter opponent abbreviations work but a few read ambiguously ("SAf" / "South Korea" both starting "S"). |
| Aesthetic | 4 | **4** | Clean grid, W/D/L palette is canonical football. |
| Novelty | 3 | **3** | Form strips are common in football reporting; 48 simultaneously is a quantity story, not a craft novelty. |

**Mean: 4.0** (vs self 4.2). Mild downgrade on novelty.

## Critique

A solid utility chart that lives or dies by readability of the
opponent/score text — currently borderline. The biggest missed extension is
*temporal*: you have W/D/L cells but no signal for *when* the goals fell, *how*
the result was achieved (early kill vs late save), or *what's next* (the R32
opponent that follows for advancers).

## Proposals

### IMPROVE — `v09b_form_strips_v2`
Replace 3-letter abbreviations with 3-letter ISO country codes (FIFA
codes: SAF, KOR, GER) for unambiguous reads; enlarge in-cell scoreline
to dominate the cell; add a 4th cell showing the team's R32 fixture
(grey-styled, marked "→") for the 32 advancers. Dataset: same.
Library: matplotlib.

### PIVOT — `team_points_bumpchart`
Bump chart: x = matchday (1, 2, 3), y = cumulative points (0..9), one
line per team, color = advance/eliminated, with shaded fan of all
teams in light grey behind. Reveals *trajectory* (who locked it up
early, who came from behind). Dataset: cumulative pts per matchday.
Library: matplotlib.

### COMBINE with v05 (`98ada01f` goals timeline) — `form_strips_with_minute_spark`
Replace each cell's score text with a 90' horizontal mini-timeline
showing when the team's goals (above the strip) and conceded goals
(below) fell. Each cell becomes a tiny match story. Dataset: per-team
goal-events. Library: matplotlib (nested axes).

### EXTEND — `signed_gd_strips`
Same 48-grid but each cell's *width* (not just color) ∝ |GD| in that
match, so "5-0 vs 1-0" reads visibly. Cell color still W/D/L. Surfaces
the dominant-vs-narrow-win distinction one-glance. Dataset: same.
Library: matplotlib custom rectangles.
