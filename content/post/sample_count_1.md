---
title: "How Much Should You Trust the Sample Count?"
date: 2020-07-08
tags: ["election", "sample count"]
categories: ["general"]
draft: false
---

Election season is upon us again here in Singapore and Polling Day is this Friday.

The last election in 2015 introduced the sample count. What is the sample count? [From the ELD Website](https://www.eld.gov.sg/mediarelease/SampleCount_Generic.pdf):

From the votes cast at each polling station, a counting assistant picks up a random bundle of 100 ballot papers (in front of the candidates and counting agents present) and counts the number of votes for each candidate (or group of candidates in the case of a GRC).

The votes will be added up, with weightage given to account for the difference in the number of votes cast at each polling station.

Sample count for the electoral division will be shown as a percentage of valid votes garnered by each candidate (or group of candidates).

The next question would be how many votes are actually counted? This depends on the number of polling stations in a SMC/GRC as only 100 ballot papers are randomly sampled from each polling station.

[There’s a very helpful article written back in 2015 by Ngiam Shih-Tung](https://www.theonlinecitizen.com/2015/09/19/how-accurate-is-the-ge-sample-vote-count/) which actually gives more helpful details. SMCs have 5-11 polling stations, with an average of 9, while GRCs have 32-66 polling stations with an average of 45. This actually significantly affects the uncertainty of the sample count.

If you’ve read the article and wonder how Shih-Tung arrived at the figures, here’s the explanation:

If we assume that votes are binary (discounting spoilt votes), then this is simply a binomial distribution sampling. Using the standard normal distribution, for a 95% confidence interval, this is 1.96 times the standard deviation, which is:

![Elo Formula](/images/std_binomial.png)

Independent of n, the number of samples, the sample standard deviation is the largest when the sample probability is 0.5. The other thing of note is the standard deviation decreases as n increases. Thus, we will expect higher error margins with SMCs compared to GRCs.

So, taking the example of Potong Pasir, the maximum standard deviation would be obtained by using p = 0.5 and n = 500, which is 0.02236. Multiplying that by 1.96 gives 0.0438, or 4.38%.

This means that if the vote is dead even, the sample count has a 95% chance of falling within 45.62%-54.38%. In contrast, for Pasir Ris-Punggol GRC, this interval is a lot narrower at roughly 48.8%-51.2%.

Hence, it is important to know how many polling stations and hence the number of samples to determine the margin of error to interpret the sample count results. If you look at table 2 of the article, you can see that in most cases, the result is a foregone conclusion after the sample count.

