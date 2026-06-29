# v07 — confederation → team treemap of group-stage goals

- Flywheel node: `5b1371e5-6e73-53d6-844f-fe85ffa843cb`
- Source repo:   `boxwheel/wc2026-viz-tournament-flow` @ `0aa44b0`
- Plot module:   `plots/v07_treemap_goals_by_team.py`

## Re-score (1–5)

| Axis | Self | Re-score | Note |
|---|---|---|---|
| Meaning | 4 | **4** | Macro distribution of attack across confs — agrees. |
| Honesty | 4 | **3** | The "alpha = goal total" overlay duplicates the area encoding redundantly *and* introduces a second visual channel that reads as "intensity"; a small-tile team (e.g., Iran) reads more saturated than expected. Area + alpha for the same quantity is a small but real honesty cost. |
| Clarity | 4 | **3** | Sub-2-goal tiles (Ghana, Tunisia, Costa Rica, Honduras-tier teams) are unlabeled or label-truncated; Panama's 0-goal tile is *invisible*, contradicting the footer claim. |
| Aesthetic | 4 | **4** | Conf-block layout reads cleanly. |
| Novelty | 4 | **3** | Hierarchical treemap is a common viz form. |

**Mean: 3.4** (vs self 4.0). Disagree on honesty/clarity/novelty.

## Critique

The chart promises a hierarchical "who scored" but loses the long tail of
small scorers (and the 0-scorer headline). The double-encoding of area + alpha
on the same metric is unnecessary and slightly misleading — area already
encodes goals. Panama being invisible because Panama scored 0 is a textbook
treemap trap.

## Proposals

### IMPROVE — `v07b_treemap_v2`
Drop alpha as a goal-magnitude channel (use a single fill per conf,
maybe with a faint border emphasis for top-3 scorers per conf). Render
0-goal teams (Panama) as a small visible black tile with white text
("Panama · 0") in a dedicated "did not score" strip below the treemap.
Ensure every tile has a min-label height (use `squarify` with explicit
text-fit guards). Library: matplotlib + `squarify`.

### PIVOT — `conf_scoring_stacked_bar`
Replace treemap with one horizontal stacked bar per conf, length =
total conf goals, segments = team contribution. Lossless re-ranking by
conf total + visible long-tail of small scorers; same data; far higher
data:ink. Library: matplotlib stacked h-bars.

### COMBINE with v11 (`4e7860c0` Elo vs pts) — `treemap_with_outperformance_border`
Color each team tile's *border* by whether they over/under-performed
Elo (gold = over, red = under). Fuses "who scored" with "who overperformed".
Dataset: treemap aggregates + v11 residuals. Library: matplotlib.

### EXTEND — `players_treemap`
Drill one level deeper: conf → team → top scorer (player). Use
`squads_and_players.csv` joined to `match_events.csv` (goal-scorer
field). Reveals the player-level mass behind each team tile. Library:
matplotlib + `squarify` (3-level).
