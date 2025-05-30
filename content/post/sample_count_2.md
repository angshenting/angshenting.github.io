---
title: "How Much Should You Trust the Sample Count?"
date: 2020-07-08
tags: ["election", "sample count"]
categories: ["general"]
draft: false
---

(Note: This was originally posted on Facebook)

TL;DR While the sample counts of this year’s GE saw some huge deviations, they are mostly within expectation.

The two hour extension brought about some online groans from friends who didn’t want to stay up late to follow the results. To help alleviate some of this anxiety, [I decided to set up a tracking sheet for the sample counts with estimated win probabilities to let people decide if the final count was worth waiting for.](https://docs.google.com/spreadsheets/d/198pBnso8oFpNSB2vNOSi-Cv5ZtIZl7lPvdk0_syZd6M/)

After the sample counts were in, there were a few constituencies left in play: Marymount, West Coast, Bukit Panjang, Sengkang and Bukit Batok. Even this is generous - the latter 3 were already >99.99% certain by my estimate. Probably the most uncertain event was whether the PVP team in Pasir Ris-Punggol would get to keep their deposit.

What was interesting as the actual counts came in were the large deviations from the sample count seen in some constituencies - Kebun Bahru was 5% off, for example. Moreover, the large deviations were mostly against the PAP. This sparked off some discussions amongst my friends.

# Analysis
My first attempt was to set up two-sided hypothesis tests (using normal approximation) if the sample proportion equals the actual proportion (Sheet “Two-sided Normal Hypothesis Test” in the link above). At the 5% level, there are a number of significant findings (note: the three-sided battles each have their own p-value for their proportion as these are not symmetric unlike the direct contests.) Kebun Bahru and Sembawang have P-values of 0.01 or less.

What if we want to test for a deviation just against the PAP vote share, i.e. the alternative hypothesis that the actual vote share is less than the sample count proportion? This would be a one-sided hypothesis test, and now Pioneer also has a P-value of less than 0.01.

## Correcting for Multiple Testing
Getting one or more statistically significant result out of 30+ does not necessarily mean there is an issue. After all, there is a 5% chance of an individual test being statistically significant just by chance. A popular method to correct for this is the False Discovery Rate (FDR) procedure, by Benjamini and Hochberg.

![Elo Formula](/images/bh_fdr.png)

Using this procedure for both two-sided and one-sided tests yields only Kebun Bahru as being statistically significant, and only in the case of the one-sided test.

A better model - incomplete beta
Yong Sheng tried using an incomplete beta distribution, which is an exact model (essentially: “draw n samples from a Bernoulli trial with probability of success p.  What is the probability that of getting > k samples?” - this is one-sided). Applying the FDR procedure to the p-values of the incomplete beta, again only Kebun Bahru was statistically significant.

What does this all mean?
Perfectly random mixing is hard in real life. From various friends who volunteered (or “volunteered”), the boxes are emptied and then mixed on the table before the lucky 100 samples are taken. To quote one of them: “We play mahjong”.

This year’s election is somewhat special - senior citizens were encouraged to vote in the morning and the younger electorate would stick to assigned time slots in the afternoon. In practice, this would mean that the ballots at the bottom of the box are at the top of the pile of the table after emptying. Might this hypothesis be true? Maybe, but we have no way of proving it. Neither should this take anything away from the best efforts of the officials involved in the counting, who faced an even harder task this year.

While the presence of large deviations might be alarming, the analysis actually shows that these are within reasonable expectation and the sample counts are reliable.

(Thanks to the various other friends involved in the discussions on this, and thanks to anyone reading this who were involved in Polling Day, you guys are the real unsung heroes of democracy!)

