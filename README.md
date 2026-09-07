# World Cup 2026 attackers: efficiency, volume, and who actually carried

This project starts from a popular but flawed World Cup 2026 attacker chart,
rebuilds it honestly, and then argues that no single view names who carried a
team. It answers two separate questions, how efficient a player was and how much
he did, states the blind spots of each, and ranks the top five on both.

Full analysis and charts are in
[`wc2026_full_analysis.ipynb`](wc2026_full_analysis.ipynb), which GitHub renders
inline.

## The argument

A metric can claim to show one thing while measuring another. The original chart
labeled its vertical axis "finishing" but blended goals and xG, which tracks shot
volume, not finishing. The rebuild separates the questions:

1. **Efficiency** (per 90): finishing as goals minus xG, creation as xA. Rewards
   rate, but cannot see workload, a rested rotation player can post a strong rate
   on a light load.
2. **Volume** (totals): a six-metric radar of each quarter-finalist's top
   attacker. Rewards output and durability, but cannot see quality, a wasteful
   high-volume shooter still looks big.

Because each view has a real blind spot, the top five is ranked on both
separately rather than merged into one score (which would double-count creation
and force an arbitrary weighting). The players on both lists are the standouts.

## What the data says

- Efficiency: Messi leads creation by a wide margin; his finishing is
  above-average, not elite (sixth).
- Volume: Messi's radar is a near-full hexagon, the most complete of any team's
  best attacker; he leads chances created (25) by a distance.
- Both rankings: Messi tops each. Mbappe second on both. Dembele and Olise also
  appear on both. Those four are the genuine standouts.

Messi carried Argentina on both the how-well and the how-much. Whether that makes
him the tournament's best player is a judgment no data settles.

## Data

Finishing (goals, xG, shots): Opta / FIFA. Creation (xA, chances created),
dribbles, big chances created: FotMob. Crosses: Opta. Efficiency uses a
350-minute floor; volume uses the top attacker per quarter-finalist. One
tournament, so rate figures carry real uncertainty; metric and weighting choices
are stated so they can be challenged.

## Stack

Python, pandas, NumPy, matplotlib.

```
pip install pandas numpy matplotlib jupyterlab
jupyter lab wc2026_full_analysis.ipynb
```
