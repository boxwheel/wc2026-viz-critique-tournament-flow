# v02 — Group → Knockout Sankey

- Flywheel node: `e13bad86-5104-5d72-8171-1d03e8b50e13`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `8903a01`
- Plot module:   `plots/v02_group_to_knockout_sankey.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 5 | **5** | The 3rd-place cut IS the structural drama of the new 48-team format. |
| Honesty | 4 | **4** | Right-side bin heights correctly proportional to team counts; uniform per-team ribbon width is honest as a count map (not a weighted flow). |
| Clarity | 4 | **3** | 48 thin ribbons crossing in the middle is visual noise; tracing a specific team is genuinely hard. The bottom-caption listing of survivors is what carries the message — the diagram is decorative. |
| Aesthetic | 4 | **4** | Restrained palette; right-side bins land cleanly. |
| Novelty | 4 | **4** | Sankey for tournament-format flow is uncommon — agrees. |

**Mean: 4.0** (vs self 4.2).

## Critique

The chart's strength is its frame (12→32 with the 3rd-place valve), but the
ribbon mass in the middle is unreadable per-team. The eye does the work the
chart promised to do once you read the caption — at which point the diagram is
ornamental. The big missed move: ribbons aren't *sorted within bin*, so a
group's four ribbons enter the middle at the same height and fan out chaotically.

## Proposals

### IMPROVE — `v02b_sankey_lane_sorted`
Sort each group's four exit ribbons by destination (Winner → Runner-up →
Best-3rd → Eliminated) so ribbons leave each group block in lane order;
that alone should drop ribbon-crossing by ~half. Color ribbons by source
group rank (1/2/3/4) not by destination, so the bottom-up qualification
story (every 3rd-place is a story; every winner is automatic) reads
visually. Dataset: same `group_standings` + R32 set-membership.
Library: matplotlib Bezier patches.

### PIVOT — `qualification_stripe`
Drop the Sankey and render one vertical "ladder" of 48 teams sorted by
points-then-GD, with three horizontal cut-lines (top-12 winners, top-24
runners-up, top-32 best-3rd). Each team a labeled tick; advancers
colored. Reveals the *exact* cutline ambiguity (e.g., South Korea cut at
3pts/-1 vs Bosnia at 3pts/+0). Dataset: `group_standings`. Library:
matplotlib stripplot with horizontal threshold lines.

### COMBINE with v09 (`686de29a` form strips) — `sankey_with_strips`
Replace each group's text-only team list (currently "Mexico / South
Africa / South Korea / Czechia") with each team's v09 3-cell W/D/L
strip. The left side of the diagram becomes a 48-strip mosaic that
*shows the path*, not just names. Dataset: v09's form frame + v02's
flow. Library: matplotlib with nested axes.

### EXTEND — `r32_to_final_sankey`
Once R32 plays out, render a second-stage Sankey from R32 entrants to
Final/Out, colored by confederation, sized by Elo. This builds a
companion piece for the knockout half of the tournament; the v02 viz
becomes the first half of a diptych. Dataset: `matches_detailed.csv`
filtered to knockout stages. Library: matplotlib.
