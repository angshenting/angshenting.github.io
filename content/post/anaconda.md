---
title: "You Should not be Using Anaconda"
date: 2024-08-01
tags: ["python", "data science"]
categories: ["general"]
draft: false
---


Imagine you’re a newcomer to Python and data analytics and some website tells you to use conda. Days later, you get an email from Anaconda telling you that you’re in breach of their licensing terms because the organisation you’re working for has more than 200 employees!

Confused, you do a quick google and find this: https://www.anaconda.com/blog/is-conda-free

Now you’re even more confused.

Meanwhile on the second search result, the answer is clearer: https://stackoverflow.com/questions/74762863/are-conda-miniconda-and-anaconda-free-to-use-and-open-source

If something named “default” is not really free unconditionally, perhaps the software isn’t free.

If a company releases something for free, then changes the terms suddenly to make it no longer free, then it just creates legal risks for people to continue using it.

If the licensing terms make it such that 1 employee in a 1000 strong organisation is on the hook for using conda, but all 199 employees in a 199 strong organisation is not in breach, then whoever decided on these regulations is not being smart about this.

(Yeah I get it, the company needs to make money, but there are better ways of licensing that better correlates with usage than a blunt instrument of company size as a cut-off.)

Most importantly: Conda doesn’t necessarily solve all of the issues associated with using pip, and it is better in the long-term for someone new to Python to learn how to use pip anyway. There might be situations where conda can’t be/should not be used.

Conclusion: Just use pip. Do not use conda.


