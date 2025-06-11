---
title: "Rating Systems (3): TrueSkill - Multiplayer Ratings"
date: 2025-05-09
tags: ["ratings"]
categories: ["bridge"]
draft: false
math: true
---

# Teams of Varying Sizes

TrueSkill was first developed by Microsoft Research for use in multiplayer team games, like Halo. It is a Bayesian ranking system which is suited for:

- **Teams** of varying sizes
- **Multiplayer** games
- **Dynamic updating** after each match

# Mathematics of TrueSkill

Each player $i$ is modeled with a skill represented by a normal distribution:

$$
\theta_i \sim \mathcal{N}(\mu_i, \sigma_i^2)
$$

- $\mu_i$ = current estimate of player skill (mean)
- $\sigma_i = uncertainty about that skill (standard deviation)

At the start, players are typically initialized with:

$$
\mu_0 = 25,\quad \sigma_0 = \frac{25}{3} \approx 8.33
$$

## Match Outcomes

Given the skills of players or teams, the *performance* in a match is drawn from:

$$
p_i \sim \mathcal{N}(\mu_i, \sigma_i^2 + \beta^2)
$$

where $ \beta $ controls the variance of performance — modeling "luck".

The outcome of a match constrains the latent performance variables:

$$
p_i > p_j \quad \text{(if player i beats player j)}
$$

TrueSkill performs Bayesian inference to update the posterior distributions of $ \mu_i $ and $ \sigma_i $ accordingly.

---

## Updating Ratings

Given observed match outcomes, TrueSkill uses the following update formulas (simplified):

### Mean update:

$$
\mu_i' = \mu_i + \frac{\sigma_i^2}{c} \cdot v
$$

### Variance update:

$$
\sigma_i'^2 = \sigma_i^2 \cdot \left( 1 - \frac{\sigma_i^2}{c^2} \cdot w \right)
$$

Where:

- $ c $ is a function of combined uncertainties.
- $ v $ and $ w $ are derived from the cumulative density function (CDF) and probability density function (PDF) of the standard normal distribution, depending on the margin of victory.

These formulas propagate uncertainty reduction over time — strong repeated performance will lower $ \sigma $, making the rating more confident.

## Handling of Teams

- Each team is modelled as the sum of its players' skills.
- Players within the same team update jointly after a match.
- Any number of players per team is supported.

---

# Comparison with Glicko and Elo

| System      | Uncertainty model | Teams support | Multiplayer | Draw support |
|-------------|-------------------|---------------|-------------|--------------|
| Elo         | No                | No            | No          | Yes          |
| Glicko-2    | Yes               | Limited       | No          | Yes          |
| TrueSkill   | Yes               | Yes           | Yes         | Yes          |

---

# OpenSkill

**[OpenSkill](https://openskill.me)** is an open-source Python package which implements TrueSkill
