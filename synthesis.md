# Synthesis — Tournament-Flow critique queue (top 5 build proposals)

Ranked by leverage = (rubric-deficit fixed) × (build cost low) × (downstream
reuse across the family).

## #1 — IMPROVE `v10b_venues_map_fixed` (honesty failure)

**Source:** v10 venues map (`4f4215b3`). Re-score 2.6 (worst of 10).

**Why:** v10 is the only chart in the family that *fails* the honesty rubric.
Three independent flaws — horizontal-band "country" backgrounds, label
truncation that prints "0k" for 70k stadiums, and a summary/body mismatch
about top-goalscoring venues — make it actively misleading. Fix-or-pull is
not optional; this is the family's load-bearing dishonesty.

**Spec:** replace bands with a real `geopandas` Natural-Earth basemap; fix
the `cap_k` formatter; reconcile summary/body via a single derived ranking;
push east-coast labels with `adjustText`.

## #2 — COMBINE `groups_canonical_page` (v01 × v04)

**Sources:** v01 group small-multiples (`f29e3669`) + v04 H2H matrices
(`98e67e2b`).

**Why:** Both viz are independently good — v04 is the family's highest-
scoring chart at 4.4. Stacking them per group (matrix above, diverging bars
below) creates *one* canonical "group page" that answers both "every game"
and "the verdict" in a single artifact, replacing two and improving both.
Highest cross-viz reuse leverage in the family.

**Spec:** matplotlib `subplot_mosaic`, 12 group panels each with two stacked
axes; shared rank-dot palette; one shared color legend.

## #3 — IMPROVE `v05b_goals_timeline_v2` (honesty fix + clarity gain)

**Source:** v05 goals timeline calendar (`98ada01f`). Re-score 3.8.

**Why:** The headline finding (26% of goals after 75', clear late spike) is
the most surprising single finding in the wave-1 family. The current viz
literally exaggerates it by clamping 90+x stoppage goals to y=90, creating
a fake wall — and clutters the cloud with a 6-color confederation legend
that pays off nothing. Fixing both is one afternoon and raises this from a
3.8 to a 4.6.

**Spec:** parse `90+x` minutes from `match_events.csv`, extend y to 105
with a shaded stoppage band; drop conf color; 1-minute marginal bins.

## #4 — IMPROVE `v06b_upset_map_v2` (labels + fit) and its DIPTYCH with v11

**Sources:** v06 upset map (`7a22b8a3`) + v11 Elo vs pts (`4e7860c0`).

**Why:** v06 has a strong concept (a 2D space that *defines* upset) ruined
by label overlap at the headline cluster of 5 outright upsets, plus an
unsupported "80 Elo ≈ 1 goal" heuristic line. v11 has a strong finding
(Mexico +4.8 / Uruguay −4.0) muddied by OLS-on-integer-y and overlapping
labels. Fixing both AND placing them as a match-vs-team-grain diptych
(`elo_outliers_diptych`) gives the family one canonical "did the favorites
hold" story.

**Spec:** `adjustText` everywhere, real OLS fit on v06 with ±1σ band,
Elo-derived expected pts on v11, gridspec two-panel composite.

## #5 — EXTEND `team_travel_paths` (new viz from v10's geography)

**Source:** new viz parented on v10 venues + v04 H2H + v11 Elo.

**Why:** Once v10 has a real basemap, the highest-value new derivative is
team travel paths — each team's three group matches as path-lines across
the basemap, total km on a side bar. WC-2026 is the first 3-country WC,
making schedule asymmetry a genuine story. This unlocks a new node *and*
forces v10 to be honest first; a healthy chain.

**Spec:** join team → matches → venues → lat/long; `cartopy` path lines
colored by team; side-bar of total km ranked.

## Cross-cutting moves the build wave should consider

- **The "canonical group page" pattern (#2) is generalisable** — once
  proved on v01+v04, the same compositing approach applies to
  v06+v11 (#4) and to v05+v09 (`clutch_form_strips`). The build wave
  should land #2 first, then port the pattern.
- **Three of the family's flaws share a root cause: matplotlib label
  collisions.** v06, v10, v11 all need `adjustText`. Ship a small
  `transforms/annotate.py` helper that wraps `adjustText` with the
  house style — once. Then re-render all three. (This single shared
  helper unlocks 3 IMPROVE proposals at once.)
- **The team-grain residual ranking is missing from the family.** v06
  and v11 both have it implicit; neither shows it as a bar chart.
  `over_under_residual_bar` (PIVOT of v11) is the single cheapest new
  artifact in the family — 30 LoC, fully buildable from existing
  derived frames.
