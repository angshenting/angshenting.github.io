---
title: "SCBA Ratings - Now Live"
date: 2025-05-22
tags: ["announcement", "bridge", "ratings"]
categories: ["general"]
draft: false
---

[The SCBA Ratings page is live!](https://dev1.scba.org.sg/ratings)

Thus far, only matchpoint pairs games are rated, from 2023 onwards. Most of the development was done in a week with the help of ChatGPT and Claude.

## Why 2023?

Given that SCBA re-opened in May 2022 after COVID-19 restrictions were eased, 2023 seems like a good starting point when most people returned to weekly games.

## How is this done?

Ratings are implemented using [OpenSkill](https://openskill.me/en/stable/), using Plackett-Luce based on rankings.

## What about teams?

In the process of implementation, most likely using Glicko-2 ([see previous post](/post/rating_2_glicko)) on head to head matches with weighting for number of boards.

