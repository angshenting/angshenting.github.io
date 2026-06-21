---
title: "Lessons from building a second brain (2) - Curating memories and data"
date: 2026-06-19
tags: ["productivity", "pkm", "obsidian", "second brain"]
categories: ["general"]
draft: false
---

# Data is the base of everything
In the first part, I talked about why I kept the design simple and why I want to be in control of the data. The implication of this is that I will also have to curate the memories and data I feed in.

Remembering people is an important part of a second brain. After all, the connections feeding the graph provide rich and valuable data. So, this means I have to curate.

## Telegram
My first attempt was to populate using Telegram contacts. However, after filtering for recency and number of messages, there were less than 100 contacts. I'm not a social butterfly after all.

## LinkedIn
I asked Claude, what if we look at LinkedIn then? After downloading my data, I then got the .csv file imported in and now I have the task of inputting some basic information of how I know every contact.

Plot twist: I can't remember how I connected with more than 30% of my contacts on LinkedIn.

## The temptation to fill in the blanks
So now I have a decision to make for every contact I can't place. I could guess. We were probably connected at a conference or event, a mutual friend introduced us or even could just be a random add that I decided to accept. I could even ask Claude to infer it — shared companies, overlapping connections, the year we connected — and write something plausible into the record.

It would have been easy. And it would have quietly broken the whole thing.

A fabricated "we met at a fintech meetup in 2023" is indistinguishable from a real one once it sits in the database. It looks like data. It reads like a memory. But it's fiction, and six months from now when Claude pulls it up to brief me before a meeting, I'll have no way of knowing it was something I invented to fill a blank.

This is the exact failure mode I built SecondST to avoid. In part 1, the whole reason I went with the boring architecture — SQLite, a read-only sync, no autonomous writes — was so I could *trust* what the system told me. SQLite doesn't drift. FTS5 doesn't hallucinate. The moment I start guessing how I know people, I'm the one introducing the drift.

So the unknowns stay unknown. The field literally says `unknown`. That's not a failure state. It's the most accurate thing I can record, and accuracy is the entire point.

## Curation is subtraction, not collection
This is where I had to unlearn something.

The instinct with a second brain is to pour everything in. Every contact, every message, every saved article. Completeness *feels* like power — surely a brain that remembers more is a better brain.

But it isn't, and the LinkedIn export was the clearest demonstration. A graph full of people I can't vouch for isn't a richer graph, it's a noisier one. And noise costs you twice: once in storage, and again every single time the AI reasons over it and confidently surfaces something irrelevant — or worse, something wrong that I can no longer distinguish from something true.

The value of the graph was never in how many nodes it has. It's in how many edges I can actually stand behind. A hundred people I genuinely know beats a thousand I don't.

So curation, it turns out, is mostly an act of subtraction. Deciding what *not* to keep. Leaving the blanks blank.

## Garbage in, garbage out — but worse
We've all heard "garbage in, garbage out." With a second brain it's sharper than the usual version, because there's an LLM sitting on top reasoning over everything I feed it.

A bad row in a spreadsheet just sits there being wrong. A bad memory in a system Claude queries gets *amplified* — it becomes the premise of an answer, the basis for a recommendation, the reason I walk into a meeting believing something that never happened. The intelligence layer doesn't sanitise bad data. It launders it into something that sounds authoritative.

Which means the curation work isn't a chore I do before the interesting AI part. It *is* the interesting AI part. The quality of everything Claude can do for me is capped by the quality of what I let into the database.

## Where this leaves part 2
Part 1 was about controlling the *system* — knowing what ran, when, and why. Part 2 is about controlling what goes *into* it, and being honest when I can't.

The principle generalises beyond contacts. It's the same question whether I'm importing LinkedIn connections, archiving messages, or saving an article to read later: **can I vouch for this, and do I know where it came from?** If yes, it earns its place. If no, I either label it honestly — `unknown` — or I leave it out. There's no third option where I quietly make something up and hope it's right.

A second brain isn't a hoard. It's a collection I'm willing to be accountable for. The unknowns on my LinkedIn list are still unknown, and I've made my peace with that. An honest blank is worth more than a confident lie.
