---
title: "Rating Systems (2): Glicko-2"
date: 2020-10-04
tags: ["ratings"]
categories: ["bridge"]
draft: false
---

# Point vs Interval Estimates
One issue with the Elo system as previously raised is how it doesn’t give any information about the uncertainty of the estimate. The Glicko system is an attempt to encompass this information additionally.

Think about this: If a player has played 3 games: win against a player rated 1500, lose against a player rated 1450 and win against a player rated 1200, how confident are we in the derived rating? On the other hand, if a player has played 50 games, and has won 35 games against opponents rated 2000 and below, drew 5 games against opponents rated 2000-2050 and lost the remaining 10 games versus opponents rated above 2050, we can be fairly certain that his rating is somewhere between 2000-2050.

And of course we now also have to take into consideration the opponent’s rating uncertainty too.

# The Glicko-2 Algorithm
[The full details can be found in this tutorial by Mark Glickman himself.](http://www.glicko.net/glicko/glicko2.pdf) I’ll run through a summary of the algorithm

Decide on two system parameters - volatility and constraint constant. Initialize rating to 1500, rating deviation (RD) to 350 and player volatility to 0.06.

Convert rating and RD to Glicko scale. Normalize rating to 0, and divide both by 173.7178. We will consider a rating period where a player meets a number of opponents with respective ratings and RDs.

- Compute $v$, which is an estimated variance of the player’s rating based on game outcomes only.
- Compute $\delta$, which is estimated improvement in rating by comparing pre-period rating vs performance rating based only on game outcomes.
- Determine the new value of volatility. This step requires numerical iterative methods.
- Update the rating deviation to the new pre-rating period value.
- Update the rating and RD.
- Convert back to original rating scale (i.e. normalised to 1500)

If a player does not play during a period, then only increase the RD according to the volatility constant.

# Advantages
Measure of volatility/uncertainty

Inactivity is “penalized” with increased uncertainty

Doesn’t rely on constant K - avoids issues of K being set wrongly

# Disadvantages
Computationally complicated

Not all players can understand two numbers instead of one for rating.

Glicko-2 is very popular - it is used on both chess.com and lichess.com. It is also the base for rating systems in DotA, CS:GO and other esports titles.