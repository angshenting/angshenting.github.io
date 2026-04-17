---
title: "A Year of Coding with AI"
date: 2026-04-14
tags: ["ai", "software engineering", "productivity"]
categories: ["general"]
draft: false
---

# Introduction

In my [2025 in Review](/post/2025_in_review/), I noted that 2025 was the year I switched to AI-assisted coding, and that organizations which embrace it will push far ahead of those which don't. That was written in late December, with the ink still wet on the experience. Four months later, with a few more projects under the belt, it's time to revisit that claim.

Three projects form the evidence base:

- **Bridge Ratings** - (May 2025 - presented at Data Science SG in Jun 2025)
- **Admissions Prediction** - straightforward binary classification, but with shifting requirements
- **Vaultshift** - started January 2026, a bridge results platform that has grown from a single-session MVP into a full multi-session system integrated with automated masterpointing and ratings from above

It's interesting to see how things have changed really fast over the past year.

# Project 1: Bridge Ratings

This started out as a mini-site by itself, using what I've previously experimented with Trueskill. Simple Leaderboard UI and a backend that integrated with the existing masterpoints system. I used Claude and GPT to generate the code and then manually copy-pasted into the created files. A lot of manual tweaking, looking at the diffs, and also setting up the project directory, but it worked. 

**What worked:** Even with the clunky workflow of generating code in a chat window and copy-pasting it into files, a well-scoped problem with a clear domain got built much faster than it would have otherwise. The AI could produce working code for a leaderboard UI and backend integration without needing much hand-holding on the logic - I'd already done the thinking in the earlier Elo/Glicko/TrueSkill posts.

**The lesson:** A tight, well-understood spec is the prerequisite. The AI tooling was primitive - no project context, no file access, lots of manual orchestration - but it still delivered. Looking back from how I work on Vaultshift now, this was the bicycle stage. It worked, but you can see the seams.

# Project 2: The admissions project - spec-as-source

A straightforward binary classification workflow. Small scope, well-defined problem. What made it interesting wasn't the modelling - it was that the business kept giving new information and requirements.

The first couple of times the spec moved, I used Claude Code to make edits. But then came some changes that was probably easier with a whole re-work. It was *cheaper to regenerate the code from the new spec than to modify the existing code*. The scope was small enough, and the AI was good enough, that "throw it away and rebuild" beat "refactor."

That changed how I thought about the artefacts. The code wasn't the thing I was maintaining. The **spec** was the thing I was maintaining - the prompt, how to process and clean each column of the data, the problem description, the evaluation criteria. The code was a build output.

**The lesson:** When code becomes cheap, especially in terms of time costs for small, well-defined projects, the durable artefact stops being the code and becomes the spec. This isn't true for every project though. I didn't realise the idea of spec-as-code until last week.

# Project 3: Vaultshift - the living system

The ratings system was really a precursor to Vaultshift. It started as an MVP to process a single session of bridge results. It is now a multi-session platform with automated masterpointing and integrated ratings. Features accreted. Integration points multiplied. Neither a greenfield nor a throwaway.

**Where AI leverage showed up:**
- Spec Driven Development and Test Driven Development is pretty much a must
- Most of the building itself - UI, data flows, scaffolding, tests
- Glue code between subsystems
- Exploring parts of the stack I was less familiar with
- Suggestions on deployment and hosting
- Refactors that would have been tedious by hand

**Where it didn't:**

- **Domain judgment.** Bridge scoring and masterpointing rules are fiddly. The AI happily writes plausible-looking code that is subtly wrong. Even feeding in the relevant documents (e.g. Masterpoints handbook), I still had to manually test on top of the generated tests. 
- **Integration contracts.** When Vaultshift talks to the ratings system or the masterpoints system, the AI can write the glue - but it cannot tell you whether the glue is *correct*. That's still on me to test and review.
- **Architecture decisions as scope grew.** The big "should this be one service or two, should this data model bend this way or that" calls still require someone holding the whole system in their head. More importantly, does it make sense to the overall product vision?

**The lesson:** Even in a growing system, AI leverage is real - but the human's job shifts. Less writing code, more owning the spec, the architecture, and the domain correctness. The AI becomes an extremely fast implementer who has no taste and no domain knowledge. Your job is to supply both.

You can see it for yourself. It's at www.vaultshift.org

# Synthesis

Three projects, three lessons:

| Project type | What AI does well | What the human owns | Durable artefact |
|---|---|---|---|
| Scoped greenfield | Almost everything | Spec, acceptance | Code *and* spec |
| Small, shifting requirements | Full regeneration | Spec, evaluation | **Spec only** |
| Growing living system | Implementation, glue, refactor | Architecture, domain, integration correctness | Code, architecture, spec |

There's also a tooling evolution across the three projects: Bridge Ratings was chat-window-and-copy-paste. Admissions used Claude Code for in-place edits. Vaultshift is full spec-driven and test-driven development with an AI agent that has project context. The workflow matured alongside the projects.

The through-line: as AI does more of the *writing*, the human's work concentrates on the *specification* and the *judgment*. The code matters less than it used to. The thinking matters more.

# What this means

This connects to the [Five Hazardous Attitudes](/post/five_hazards/) I wrote about last year. A few things I think follow:

- **Review shifts upstream.** The important question is less "is this code correct" and more "is the spec right, and does the code match it." The second question is harder and more valuable.
- **Domain knowledge matters more, not less.** Bridge scoring rules, masterpoints handbooks, admissions criteria - the AI can't tell you these are wrong. You need people who know the domain deeply enough to catch plausible-but-wrong output.
- **Specs become load-bearing.** If the spec is the durable artefact, it has to be written down properly, version-controlled, and kept current. This is a documentation discipline most teams don't have yet.
- **The junior onramp changes.** The traditional path of learning by writing lots of code is partially automated away. What replaces it isn't obvious yet, but anecdotally self-motivated juniors are learning and improving **fast**.

# Looking ahead

The frontier is moving toward longer-horizon agentic workflows - AI that can hold a task across hours or days, not just turns. If that lands, the spec-as-source pattern will apply to bigger and bigger problems.

The productivity claim I made in December still holds. But the more interesting shift is not that I'm faster. It's that what I spend my time on has changed. The human is still the bottleneck, and even more so now, but this is mitigated by providing proper structure and software engineering practices to increase code quality in fewer tries.
