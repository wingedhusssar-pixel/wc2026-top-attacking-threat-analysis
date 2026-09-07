# An honest expected-goals chart, and the one profile that survives it

One popular World Cup 2026 player chart claimed to show finishing quality on its
vertical axis. It did not, placing inaccurate weights that shifted data. This project takes that chart apart, rebuilds it
honestly in Python, and then calibrates it: instead of trusting every point
equally, it measures how firmly each one can be read. That calibration is also
what makes the single exceptional profile in the tournament stand out for a
reason the statistics actually support.

The full analysis, code, and charts are in
[`wc2026_xg_analysis.ipynb`](wc2026_xg_analysis.ipynb), which GitHub renders
inline.

## What was wrong with the original

The source chart plotted attackers on a blended vertical axis of 0.3 x goals
plus 0.7 x xG per 90, labeled as finishing quality. Three problems:

1. **The axis measured the wrong thing.** A blend of goals and xG tracks shot
   volume and chance quality, not finishing. Real finishing is goals minus xG,
   the overperformance the blended axis buries.
2. **No creation dimension done right.** The rebuild uses xA per 90, which
   credits the quality of a chance created and does not punish a creator whose
   teammates miss.
3. **Small samples treated as equal.** A cameo substitute on 90 minutes sat next
   to a player who started every match. The rebuild applies a 270-minute floor
   and sizes each dot by minutes.

## Calibrating the axes, not discarding them

The two axes rest on different numbers of events, and that decides how firmly
each can be read.

- **Finishing rests on rare events.** A player takes only a few shots a game, so
  over five games his goals-minus-xG figure sits on a handful of chances. One
  finish or one miss swings it hard.
- **Creation rests on frequent events.** Chances created happen many times per
  game, so xA accumulates over far more events in the same minutes and steadies
  faster.

Putting a confidence band on finishing (goals modeled as Bernoulli trials, one
per shot) shows the effect: **15 of 16 players have a 95% finishing band that
crosses zero.** That does not make the finishing axis meaningless. It tells you
to read it as what happened in five games, not as settled skill. The creation
axis needs no such heavy discount.

## The profile that survives every filter

Messi is the exception, and for a reason the analysis supports rather than
contradicts. His standout number is not finishing, where uncertainty is widest,
it is creation, the axis built on the higher event count. His xA per 90 sits
alone at the top of the field, off 530 minutes, not a cameo. He occupies the
complete-attacker region, scoring above his expected goals while creating more
than any other player in the tournament, at 39. Strip out the small-sample
finishing noise and the cameo distortions, hold only the metric that survives
scrutiny, and one player is still out on his own. No five games prove a
greatest-ever claim, but the data is fully consistent with a combination the
historical record does not offer a clear second example of.

## Charts

- Finishing vs creation, minutes-weighted, no verdict quadrants
- Finishing with 95% confidence bands, showing how firmly each can be read
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
(wc2026_attackers.csv). Per-90 xA was taken from FotMob's per-90 creation stats
and cross-checked against published season totals, which agreed to within about
0.02 for most players. The finishing model treats each player's total xG as a
fixed expectation and goals as the random outcome, a standard first-order
approximation rather than a full hierarchical model.
