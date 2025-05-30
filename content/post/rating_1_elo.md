---
title: "Rating Systems (1): Elo and its limitations"
date: 2020-08-14
tags: ["ratings"]
categories: ["bridge"]
draft: false
---

# Introduction
This is going to be a new series on rating systems, which is a vastly underrated (pun intended) area of statistics and data science. Rating systems has actually been a part of my life (and probably yours), from early days in chess and then games with matchmaking like CS:GO and Valorant, to now thinking if contract bridge should also have one.

# Historical Context
Having been around for centuries, chess is a game which people have wasted much time on arguing/debating who is the best player. It’s probably slightly surprising then that the first modern rating systems only appeared around or after the end of World War 2. The first systems (Ingo and Harkness) were quite simple and used the idea of the average rating of opponents with adjustments for the results.

It is worth noting that the English Chess Federation still has a rating system still in operation (since 1958) with somewhat similar ideas: a player’s grade is his opponent’s grade +- 50 (depending on win, or loss; it’s the opponent’s grade if it’s a draw). The ECF system also caps the difference to 40 points between both players (even if the actual difference is more than 40), and the player’s rating is the average over all the matches during a time period.

# ECF System
The ECF system, while limited, actually contains some fundamental ideas:

Equally strong opponents will draw (might not be true for other games), or have an even chance of winning.

Winning results in gain in rating, losing results in a loss. The outcomes are binary.

Some form of capping is needed to prevent huge changes in rating, or even worse, higher rated players losing rating even after a win.

Averaging over a time period.

One big criticism is how the ECF rating system is a “lagging” indicator, especially for junior players who improve faster than the rating system can catch up.

# Elo System
The rating system takes its name after it’s inventor, Arpad Elo, a Hungarian-American professor of physics who was also a chess player.

## Expected Score
The main idea of the rating system is that of the expected score, which is the sum of the probability of winning plus half the probability of drawing (in chess, a score of 1 is given for a win and 0.5 for a draw). Elo suggested scaling the ratings such that a difference of 200 points would give an expected score of 0.75 for the stronger player. The average rating of 1500 was chosen by the US Chess Federation and this is used as the initial rating. Note that no distinction is made between wins and draws in this expected value; a draw is simply half a win.

The expected score is simply a logistic function, for example the expected score of Player A and Player B is given below, where R are the respective ratings:

![Elo Formula](/images/elo_formula.png)

Note that the two equations are symmetrical.

The rating is then updated by multiplying the difference between the actual and expected score by a K factor.

Elo originally set K=10, which is deemed to be too low, i.e. too insensitive/lagging behind actual performance. The FIDE tiers this to three different levels:

K = 40, for new and young players aged under 18

K = 20, for players rated below 2400

K= 10, for players rated above 2400, and at least 30 games played in previous events.

# Mathematical Issues
The normal distribution is easy to understand, and symmetrical, but in practice the US Chess Federation found the logistic distribution a better fit.

The correct value of K, as stated above.

The Elo rating is a point estimator, without any indication of uncertainty. This is problematic for players who are new or players returning from a long period of inactivity.

# Practical Issues
Players can choose not to play to protect their high rating, which is undesirable for competition.

Players might choose their choice of opponents to minimize risk and maximize rewards

