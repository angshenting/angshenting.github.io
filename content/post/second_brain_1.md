---
title: "Lessons from building a second brain (1) - Keeping it simple"
date: 2026-06-19
tags: ["productivity", "pkm", "obsidian", "second brain"]
categories: ["general"]
draft: false
---

# Keeping It Simple: Why I built my own second brain instead
Everyone's talking about a second brain/chief of staff nowadays. OpenClaw and Hermes Agent meetups here in Singapore are always full and waitlisted. I chose to build my own instead. Here's why.

---

## What everyone else is doing

OpenClaw is the rocket ship of the moment. It connects your AI to your files, your messages, your inbox, and your calendar, then hands it a set of "skills" to act on your behalf — drafting emails, clearing your inbox, submitting job applications via browser automation. It's proactive. It's always on. It's doing things while you sleep.

Hermes goes a step further. Built by Nous Research, it has a three-layer memory system — skill memory, conversational memory, and a user model — that feeds a self-improvement loop. The agent gets measurably better at recurring tasks over time. It learns you.

Both are genuinely impressive pieces of engineering. Both are also solving a problem I'm not sure I have.

---

## The problem with agents that act

My first reaction when I first came across OpenClaw: **who's in charge?**

We've read about agents deleting entire email inboxes and production databases. Sure, there's always the promise of "human in the loop" but is the human in the right place in the loop? The promise of an always-on, autonomous agent is seductive. The reality is that these systems introduce a layer of behaviour you can't fully audit. Memory.json gets written by the agent. Skills get modified. The loop improves. At some point the thing doing the work is no longer the thing you configured. Also, cost - agents running autonomously 24/7 are going to rack up huge amounts of token costs. 

I wanted something different: **a system where I always know what ran, why it ran, and what data it touched.** 

---

## The SecondST architecture

SecondST is built on three deliberately separate pieces:

A local SQLite database. It ingests Telegram messages, Gmail, Google Calendar, and LinkedIn connections into a unified schema. It never writes back to those services. It's read-only by design. There's nothing autonomous about it — it's a database that syncs.

The execution layer, implemented in n8n. It sends Telegram notifications, triggers workflows, acts on the world. Critically, it has no intelligence of its own. It does exactly what a workflow tells it to do, nothing more.

The intelligence layer is Claude. It queries the database via a REST API, uses the results to reason, then instructs n8n to act. It never touches the database directly. It never acts without being asked. It's stateless between sessions by design.

Even better: most of the time, I can just run a Claude Code session instead of going through API keys. If I need a loop to research, I initiate one and also set end conditions for the loop. So far, every loop I've run has terminated within 10-12 rounds.

---

## What this looks like in practice

My morning briefing is an n8n workflow that fires at 7am: it calls the SecondST API for today's calendar events, queries recent messages, and sends a Telegram digest. No LLM involved. No autonomous decisions. A deterministic workflow running on known data.

When I want to recall something — "what did Noven say about that project last month?" — I ask Claude directly. It queries the FTS5 full-text search, semantically recalls from the vector index, and gives me an answer. Then it stops. It doesn't file that query for later. It doesn't update a user model. The next session starts clean.

Compare this to OpenClaw's memory.json, which the agent writes continuously, or Hermes' user model, which shapes future responses in ways that aren't always visible. My approach is slower to start a session. It's also completely predictable.

---

## Where the tradeoffs land

I won't pretend SecondST is better than OpenClaw or Hermes on every dimension. It isn't. Here's an honest comparison:

| | SecondST | OpenClaw | Hermes |
|---|---|---|---|
| **Setup** | Hand-built over weeks | Single curl / npm install | Single curl install |
| **Proactivity** | Scheduled workflows only | Always-on agent | Always-on agent |
| **Memory** | SQLite + FTS5 + vector index | memory.json | 3-layer (skill / conv / user) |
| **Self-improvement** | None | Community skills | Built-in learning loop |
| **Autonomy** | Human-in-the-loop always | High | Very high |
| **Auditability** | Full — it's just SQL | Medium | Low |
| **Privacy** | Everything local, no telemetry | Local, but agent-managed | Local, MIT, no telemetry |

OpenClaw wins on breadth of integration and community. Hermes wins on depth of learning. SecondST wins on transparency and control.

---

## Why simplicity is the point, not a limitation

The systems that are most likely to earn long-term trust are the ones you can reason about. I can open the SQLite file and read my data. I can read the n8n workflow JSON and know exactly what runs when. I can look at the REST API response and see exactly what Claude saw before it answered.

With OpenClaw or Hermes, a significant part of the system's behaviour lives in learned state — memory.json entries the agent wrote, user models shaped by patterns it noticed. That state might be correct. It probably is, most of the time. But when it's wrong, debugging it is hard. And the longer the system runs, the more opaque it becomes.

SecondST's memory is just a database. SQLite doesn't drift. FTS5 doesn't hallucinate. Debugging is straightforward, instead of having to look into hidden states. Cost is well controlled and mostly I can just use my existing Claude subscription.

Yes, there's no official self-improvement, but SecondST is evolving, I add skills that I truly need, and I can choose to remove old skills and/or improve them. Maybe I should call it co-improvement: I'm also improving my knowledge of how agentic AI works with every new improvement I make to SecondST.

---

## Who should build what

For those starting out, OpenClaw and Hermes are great places to start. 

But if you want to know what your AI system did with your data, what it's about to do next, and why: consider building something boring. SQLite, a few API calls, a webhook into Telegram. You will have full control and understanding of your system
