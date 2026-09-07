# Rebuilding a flawed expected-goals chart

A popular World Cup 2026 player chart claimed to show finishing quality on its
vertical axis. It did not. This project takes that chart apart, rebuilds it
honestly in Python, and then quantifies the one flaw no rebuild can remove:
at tournament sample sizes, finishing is mostly noise.

The full analysis, code, and charts are in
[`wc2026_xg_analysis.ipynb`](wc2026_xg_analysis.ipynb), which GitHub renders
inline.

## What was wrong with the original

The source chart plotted attackers on a blended vertical axis of 0.3 × goals
plus 0.7 × xG per 90, labeled as finishing quality. Three problems:

1. **The axis measured the wrong thing.** A blend of goals and xG tracks shot
   volume and chance quality, not finishing. A high-volume shooter scores high on
   it whether or not he converts. Real finishing is goals minus xG, the
   overperformance the blended axis buries.
2. **No creation dimension done right.** The rebuild uses xA per 90, which credits
   the quality of a chance created and does not punish a creator whose teammates
   miss.
3. **Small samples treated as equal.** A cameo substitute on 90 minutes sat next
   to a player who started every match. The rebuild applies a 270-minute floor
   and sizes each dot by minutes.

The rebuild also drops the "finisher / creator / wasteful" quadrant labels,
which bake a verdict into a five-game sample and mislabel low-volume creators
like Olise and Wirtz as poor finishers.

## The statistical core

Goals is a sum of Bernoulli trials, one per shot, each converting with
probability equal to its xG. Treating total xG as the fixed expectation, the
standard deviation of goals minus xG is about the square root of total xG.
Over a World Cup, total xG is small, so the confidence band on finishing per 90
is wide.

The result: **15 of 16 players have a 95% finishing band that crosses zero.**
At these sample sizes, almost no player's finishing is distinguishable from
average. A minutes floor removes cameo bias, but it cannot shrink a band that a
short tournament makes wide.

## Charts

- Finishing vs creation, minutes-weighted, no verdict quadrants
- Finishing with 95% confidence bands, showing nearly all cross zero
- Confidence band width shrinking as minutes grow, tournament vs full season

## Stack

Python, pandas, NumPy, matplotlib. Open the notebook to read it with charts, or
run it:

```
pip install pandas numpy matplotlib jupyterlab
jupyter lab wc2026_xg_analysis.ipynb
```

## Data note

Figures were compiled from FotMob's World Cup 2026 player stats during analysis
(`wc2026_attackers.csv`). The finishing model treats each player's total xG as a
fixed expectation and goals as the random outcome, a standard first-order
approximation rather than a full hierarchical model.
