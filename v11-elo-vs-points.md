# v11 — Elo vs group-stage points scatter

- Flywheel node: `4e7860c0-d673-50b8-8f83-1d39a3a0923b`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `d836f29`
- Plot module:   `plots/v11_elo_vs_points.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 5 | **5** | Directly names over/under-performers — agrees. |
| Honesty | 5 | **4** | OLS regression onto an *integer* y (0..9 points) is methodologically loose — the residual scale is in "points", which only takes 10 discrete values. A logistic / multinomial fit (P(W/D/L) → expected pts) would be more honest; current linear fit understates uncertainty. Disclosure of the integer-y nature is missing. |
| Clarity | 4 | **3** | Labels in the y=4 stack (Switzerland, Norway, Côte d'Ivoire, Bosnia, South Africa all near (1700, 4)) overlap heavily; the gold/red label boxes occlude their own dots. |
| Aesthetic | 4 | **4** | Confederation palette + over/under callout boxes — fine. |
| Novelty | 4 | **3** | Elo vs points calibration is a standard sports-stats chart form. |

**Mean: 3.8** (vs self 4.4). Real disagreement on honesty + clarity + novelty.

## Critique

The headline finding (Mexico +4.8, Uruguay −4.0) is unambiguously valuable;
the chart that delivers it isn't quite there. Two cheap wins: jitter the y to
break point-stacks (with the disclosure), and use a proper expected-points
model. A bonus win: replace the scatter with a sorted residual bar that pulls
the over/under-performer story out of a 2D space.

## Proposals

### IMPROVE — `v11b_elo_vs_points_v2`
- Apply uniform jitter `y_jit = pts + uniform(-0.18, 0.18)` and disclose
  in the caption ("y jittered to break ties").
- Replace OLS-on-points with expected-points derived from per-match
  P(win/draw) under the Elo gap formula `P = 1 / (1 + 10^(-Δ/400))`,
  summed over each team's three group matches; use that as the
  expectation. Show the formula in a corner.
- Push labels with `adjustText`.
Dataset: same. Library: matplotlib + numpy + `adjustText`.

### PIVOT — `over_under_residual_bar`
Single horizontal bar chart sorted by (actual pts − expected pts) with
team names labeled. Same information; instantly answers "who
over/under-performed Elo". Dataset: per-team residuals. Library:
matplotlib h-bars with diverging color.

### COMBINE with v06 (`7a22b8a3` upset map) — `elo_outliers_diptych`
Two-panel: v11 team-grain (left) + v06 match-grain (right) with
cross-highlighting (Mexico's three wins highlighted as gold dots in
v06 when Mexico is the gold over-performer in v11). Dataset: shared.
Library: matplotlib gridspec.

### EXTEND — `post_group_elo_calibration`
Update each team's Elo *after* the three group matches (a single
Elo-K-pass update per result), plot Elo_post vs Elo_pre. Teams above
the y=x line gained rating; below, lost. Calibrates the rating system
itself against the tournament. Dataset: `teams.csv` Elo + match
results. Library: matplotlib.
