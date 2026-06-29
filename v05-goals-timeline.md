# v05 — group stage in minutes (every goal)

- Flywheel node: `98ada01f-1bae-52d2-a4dc-d0abf9e747bd`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `69cf165`
- Plot module:   `plots/v05_goals_timeline_calendar.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 5 | **5** | Late-spike pattern (26% in 75+) is real and surprising. |
| Honesty | 4 | **3** | Stoppage-time goals (`90+x`) are clipped to y=90 producing a visible flat-line stack at the top of the plot — that's a *visual lie about temporal density*. Caveat is text-only; the rendered image overstates the 90' spike. Also: confederation color in the swarm carries no testable hypothesis and adds noise. |
| Clarity | 4 | **3** | 6-confederation legend forces a chromatic lookup the chart never pays off; dots overlap at the top edge. |
| Aesthetic | 4 | **4** | Calm dot-cloud + marginal histogram is a good shape. |
| Novelty | 4 | **4** | Date × minute swarm at WC aggregate grain is uncommon. |

**Mean: 3.8** (vs self 4.2). Real disagreement on honesty due to 90' clamp.

## Critique

The headline finding (late goals dominate) is correct and undersold — that's
worth a chart all to itself. The current viz loses honesty by stacking 90+x
stoppage goals at y=90, exaggerating the visible "wall" right at 90. The
confederation coloring is also a decorative move that doesn't pay; it costs a
legend and clutters the cloud.

## Proposals

### IMPROVE — `v05b_goals_timeline_v2`
Plot stoppage-time goals at their true reported minutes (`90 + x`) and
extend y to 105, with a faint grey band marking 90–105. Drop the
confederation color (use a single ink dot or color by *scoring team's
own goal #* in that match to add a small narrative). Recompute the
marginal histogram with finer bins (1-minute) to expose the smooth
late-game rise. Dataset: `match_events.csv` parsed for `90+x`.
Library: matplotlib + seaborn `histplot`.

### PIVOT — `goal_minute_distribution_vs_history`
Drop the (date × minute) and just plot the *minute distribution* (1D
KDE/hist) of WC-2026 goals overlaid against the historical baseline
from prior WCs (use any historical minute distribution available in
public datasets). Direct visual answer to "is the late spike
distinctive?". Dataset: this + historical reference (cite source).
Library: matplotlib `kde` + reference line.

### COMBINE with v09 (`686de29a` form strips) — `clutch_form_strips`
Color each form-strip cell by whether the team's winning/losing goal
fell in min 75+. Surfaces "clutch wins" vs "early kills"
team-by-team. Dataset: `match_events.csv` last-goal-decisive
classification + v09 frame. Library: matplotlib.

### EXTEND — `late_window_top_teams`
Filter goals to minute ≥ 75, bar chart of top teams ranked by late
goals (scoring), with a parallel bar for late goals *conceded*. Names
who closed games strong and who let them slip. Dataset:
`match_events.csv` joined to `teams.csv`. Library: matplotlib h-bars.
