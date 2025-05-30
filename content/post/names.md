---
title: "Mandatory Reading on Names"
date: 2021-04-29
tags: ["data science"]
categories: ["general"]
draft: false
---
[Yes, we’re talking about actual names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/)

It’s a pity that I came across this 6 months too late, as it would have saved me an hour repeating myself thrice on why we can’t use names as a unique key to join across different data sources.

Thankfully my point eventually got across, but to any fellow developer/data scientist/engineer having to explain to stakeholders, hopefully this helps.

And no, you do not want to use email addresses as a unique key either:

People make typos with their email addresses more often than you think, e.g. “.com” instead of “.edu”

People have multiple email addresses and are inconsistent with which one they use

People change email addresses more often than you think

If there is already an existing unique ID of sort (access card, database generated, registration number), then that should be used instead.
