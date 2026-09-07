# An honest expected-goals chart, and the profile that survives every filter

A popular World Cup 2026 player chart claimed to show finishing quality on its
vertical axis. It did not. This project takes that chart apart, rebuilds it in
Python, and then calibrates it: instead of trusting every point equally, it
measures how firmly each one can be read. That discipline is what makes the one
exceptional profile in the tournament stand out for a reason the statistics
support.

Full analysis, code and charts are in
[`wc2026_xg_analysis.ipynb`](wc2026_xg_analysis.ipynb), which GitHub renders
inline.

## What was wrong with the original

The source chart plotted attackers on a blended vertical axis of 0.3 x goals
plus 0.7 x xG per 90, labeled as finishing quality. Three problems:

1. **The axis measured the wrong thing.** A blend of goals and xG tracks shot
   volume and chance quality, not finishing. Real finishing is goals minus xG,
   the overperformance the blended axis buries.
2. **No creation dimension done right.** The rebuild uses xA, which credits the
   quality of a chance created and does not punish a creator whose teammates miss.
3. **Small samples treated as equal.** The rebuild applies a 350-minute floor and
   sizes each dot by minutes, so cameo rates do not distort the picture.

## A symmetric inclusion rule

A player is in the set if he generated **xG >= 1.3 OR xA >= 1.3** and played
350+ minutes. Both gates use the same unit (expected goals), one for scoring and
one for creating, so the two sides are comparable. This keeps pure finishers who
shoot but rarely create, and pure creators who create but rarely shoot. The
gates use xG and xA, not goals and assists, because expected values are the
stabler signal over a five-game sample.

## Calibrating both axes

**Finishing** rests on rare events. Modeling goals as Bernoulli trials over each
player's real shot count gives a confidence band, and **18 of 19 bands cross
zero**: over five games, finishing is rarely distinguishable from average.

**Creation** rests on far more events, so xA is the stabler axis. Plotting xA
against actual assists confirms it is real, not teammate luck: Messi sits on the
diagonal (4.2 xA, 4 assists), his creation converted as expected.

## The profile that survives every filter

On finishing, Messi is not the standout, his overperformance ranks sixth. His
singular trait is creation: he leads xA on both totals and per-90, so the lead is
neither an artifact of playing the most minutes nor of a hot rate in a thin
sample. It survives every adjustment. He posts it while finishing above his own
xG, at 39. Not the best finisher, but a genuinely above-average one who is also,
by a wide and well-calibrated margin, the best creator in the tournament.

## Data

Finishing (goals, xG, shots, efficiency): FIFA official World Cup 2026 stats.
Creation (xA): FotMob. Each axis is single-source. Goals derived as xG times
FIFA's xG-efficiency. 19 players, 350-min floor, symmetric xG-or-xA rule.

## Stack

Python, pandas, NumPy, matplotlib. Open the notebook to read it with charts, or:

```
pip install pandas numpy matplotlib jupyterlab
jupyter lab wc2026_xg_analysis.ipynb
```
