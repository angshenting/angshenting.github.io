---
title: "Debugging the MUD I grew up in"
date: 2026-09-06
tags: ["mud", "gaming", "ai", "reverse engineering", "software engineering"]
categories: ["general"]
draft: false
---

# A world that never logged off

Twilight was an LPMUD - a text-based multiplayer world I spent hours through my teens and my early 20s. How I agonized over character deaths, getting banned by the trigger-happy arch back then. The MUD has gone through many hands, but David (with the help of ZH) has kept it alive on AWS.

The mudlib underneath it dates to 1996. The math has felt wrong, it felt wrong when David first made me a wiz during my uni days but it was just difficult to go through the codebase. Recently David started GPT-6 Astra on the old codebase, and it kept flagging how badly connected the areas were. When the copy of the code was sent over, Fable 5.1 started pointing out how badly written the skills code was. No surprises there, honestly, most of the old generation of wizzes were not particuarly great coders, nor did they have the tooling to succeed. Line by line editing using ed, anyone?

The first two bugs pointed out were not new - the more recent (and I mean 2009 recent) wizzes have all felt something was really off with the combat system, and our guts were right.

# Bug 1: stats that stack forever

The first bug lets stat modifiers stack in a way they were never supposed to. I turned a banshee - a fairly ordinary mob - into this:

```
| |-S-T-A-T-S-----------------------------------------------------------| |
| | Strength       1065 / 1065   50118 | Intelligence     1 /   1 10193824 | |
| | Dexterity        1 /   1 2149423 | Constitution   590 / 590   50118 | |
| | Hit Points     404 / 615       1 | Spell Points    26 /  26       1 | |
| |---------------------------------------------------------------------| |
```

Strength in the thousands, a dexterity modifier past two million. It's a real bug - the stacking logic clearly isn't supposed to allow this - but the honest caveat is that I only got there with Arch commands. It's not something an ordinary player can trigger from inside the game. Good to fix, not urgent, filed and moving on.

# Bug 2: resistance runs backwards

This one is live, and it's the one worth explaining properly, because once you see the shape of it you can't unsee it.

Here's what resistance is supposed to mean, and the game is not shy about telling you: positive numbers mean protection.

- A quest amulet identified as "Fire Resistance" adds 50 fire resistance while worn (`domains/Quest/torasin/mquest1/reward.c:36`).
- A brooch identified as "+10% ALL RESIST" adds 10 to acid, fire, and everything else (`domains/Moor/obj/reward.c:26`).
- `IMMUNE` is defined as 100, and `GetResistance` clamps the whole range to -100..100.
- The class table gives undead classes `death 100` and `fire -20`, matched by `#imm death` and `#vuln fire` flags in the guild table.

Every piece of authored content agrees: positive resists you, negative hurts you. Now here's what the formula actually does with that number. `eventComputeSkillDamage` in `lib/body.c:308` multiplies damage by `1 + resistance/100` when resistance is positive, and divides by `1 - resistance/100` when it's negative. Read that again. The fire resistance amulet makes you take *one and a half times* fire damage. The zombie's fire vulnerability makes it take *less* fire damage. The sign is backwards.

It gets worse, because the inverted formula doesn't run once - it runs twice on the same hit:

- `GetDamage` in `lib/spell.c:99` applies it, and then `spell.c:811` calls `eventComputeSkillDamage`, which applies it again.
- Individual spells apply it a third way and then hand off to the same body function. `fireball.c:177` does this, then calls `eventReceiveSkillDamage`, which applies the inversion once more. Fourteen other spells and skills follow the identical pattern - curse, static, cloudkill, icestorm, backstab among them.
- Prayers scale the resistance by a class factor in `lib/prayer.c:127` first, then run into the same doubled formula.

Weapon hits are untouched by any of this - `eventReceiveDamage` never looks at resistance at all. It's purely a spell-and-skill bug, which is exactly why it's gone unnoticed for so long. Nobody audits three-decade-old fireball code by eye and spots a sign error buried two function calls deep, applied twice, and scaled by a class factor on top.

What it means for an actual character: a cleric wearing the fire amulet, fireballed, takes 1.5x damage in the spell and 1.5x again in the body - 2.25x the damage of someone wearing nothing. A firedrake, whose class sheet gives it fire 20 and cold -20, takes 1.44x fire damage and roughly 0.7x cold - precisely backwards from its design. Undead with `death 100` take four times death damage from anything that doesn't explicitly check for `IMMUNE`, and as far as we can tell only `harm`, `exorcise`, and `static` bother to check.

# Making the bug happen

Reading the code gets you a hypothesis. It doesn't get you a number. So with Fable 5.1's help I built a test mob and three copies of it with different fire resistance values, and hit each one with the same spell:

- fire 0: 167 damage
- fire 100: 624 damage
- fire -100: 37 damage

| Dummy | Observed | Ratio to neutral | Prediction under current code |
|---|---|---|---|
| fire 0 | 167 | 1.00 | 1.0 |
| fire 100 | 624 | 3.74 | 4.0 |
| fire -100 | 37 | 0.22 | 0.25 |

The direction is exactly backwards - the dummy the game defines as immune took the *most* damage, and the maximally vulnerable one took the *least*. There's no reading of that data where it's intentional.

The ratios also confirm it's a double application, not a single one. One inverted multiplication would give you ratios of 2.0 and 0.5 - roughly 334 and 83 damage. What we measured, 3.74 and 0.22, sits right next to the 4.0 and 0.25 that two multiplications in sequence produce. That rules out the tempting quick fix of "just patch fireball" - the body's `eventComputeSkillDamage` is doing its own share of the damage.

They're not exactly 4x and 0.25x because `fireball` rolls `random(attack/5)` separately for each target inside its loop, so each dummy started from a slightly different base before resistance ever got applied - roughly a 13% band, and both deviations fall inside it. Integer truncation on the division path shaves a point or two off the vulnerable case as well. The noise is fully accounted for. The signal isn't in question.

# The actual hard problem

Finding the bug was the easy part, in the end. AI reading old C is good at pattern-matching "this formula's sign looks inverted against what the data says it should mean," and reproducing it in-game turned a hypothesis into three numbers on a screen. That part went fast.

The hard problem is what David and I are chewing on now: what do you do with a bug like this in a mudlib written in 1996, on a game that's been quietly live the whole time? Fixing the sign is a one-line change. But the game's balance has been running on the *broken* version for thirty years. Every class's fire/cold/death numbers, every reward item's resistance value, every build a player has ever optimised around - all of it was tuned, consciously or not, against the inverted formula. Flip the sign and you haven't just fixed a bug, you've silently reversed the effective difficulty of every resistance-bearing item and every undead class in the game overnight.

So the question isn't "is this a bug" - it clearly is - it's what a responsible fix looks like when the wrong behaviour has become load-bearing. Do you fix the formula and rebalance every number that was calibrated against it? Do you fix it only for new content and leave legacy items grandfathered? Do you version the damage pipeline so old and new coexist during a transition? None of those are code questions. They're the same questions you'd ask about any long-lived production system with real users sitting on top of undocumented behaviour - except this one has been running since before some of those users were born.

We don't have the answer yet, but it promises to be an interesting exercise in code modernization using AI.