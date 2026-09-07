# An honest expected-goals chart, and the one profile that survives it

A popular World Cup 2026 player chart claimed to show finishing quality on its
vertical axis. It did not. This project takes that chart apart, rebuilds it in
Python, and then calibrates it: instead of trusting every point equally, it
measures how firmly each one can be read. That discipline is also what makes the
single exceptional profile in the tournament stand out for a reason the
statistics support.

Full analysis, code and charts are in
[`wc2026_xg_analysis.ipynb`](wc2026_xg_analysis.ipynb), which GitHub renders
inline.

## What was wrong with the original

The source chart plotted attackers on a blended vertical axis of 0.3 x goals
plus 0.7 x xG per 90, labeled as finishing quality. Three problems:

1. **The axis measured the wrong thing.** A blend of goals and xG tracks shot
   volume and chance quality, not finishing. Real finishing is goals minus xG.
2. **No creation dimension done right.** The rebuild uses xA, which credits the
   quality of a chance created and does not punish a creator whose teammates miss.
3. **Small samples treated as equal.** The rebuild sizes each dot by shots and
   applies an output floor, so cameo samples do not distort the picture.

## The inclusion rule is symmetric

A player is in the set if he generated **xG >= 1.3 OR xA >= 1.3** across the
tournament. Both gates use the same unit (expected goals), one for scoring and
one for creating, so the two sides are directly comparable. This keeps two kinds
of player a one-sided rule would drop: pure finishers who shoot but rarely
create, and pure creators who create but rarely shoot.

The gates use xG and xA, not goals and assists, on purpose: the analysis trusts
expected values over outcomes because five-game outcomes are noisy. A player with
four assists on 0.9 xA is mostly the creation-side mirror of a lucky finisher.

## Calibrating both axes

**Finishing** rests on rare events (a few shots a game), so goals-minus-xG is
noisy. Modeling goals as Bernoulli trials over each player's real shot count
gives a confidence band, and **19 of 20 bands cross zero**: over five games,
finishing is rarely distinguishable from average.

**Creation** rests on far more events, so xA is the stabler axis. Plotting xA
against actual assists confirms it is real, not teammate luck: Messi sits on the
diagonal (4.2 xA, 4 assists), his creation converted as expected.

## The profile that survives every filter

On finishing, Messi is not the standout, his efficiency ranks sixth here. His
singular trait is creation: 4.2 xA laps a field whose next best is 3.3, and the
assist check confirms it is real. He posts it off the largest shot sample in the
set, while still finishing above his own xG, at 39. Not the best finisher, but a
genuinely above-average one who is also, by a wide and well-calibrated margin,
the best creator in the tournament.

## Data

Finishing (goals, xG, shots, efficiency): FIFA official World Cup 2026 stats.
Creation (xA): FotMob. Each axis is single-source. Goals are derived as xG times
FIFA's xG-efficiency. 20 players, symmetric xG-or-xA inclusion rule.

## Stack

Python, pandas, NumPy, matplotlib. Open the notebook to read it with charts, or:

```
pip install pandas numpy matplotlib jupyterlab
jupyter lab wc2026_xg_analysis.ipynb
```
