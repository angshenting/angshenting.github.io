---
title: "Good Governance Is Good Design"
date: 2026-04-17
tags: ["ai", "governance", "iso 42001", "nist ai rmf"]
categories: ["general"]
draft: false
---

# Introduction

Two days ago I co-moderated a roundtable on ISO 42001. Multiple questions were asking what is AI Governance: is it going to look like the worst parts of compliance - a checklist bolted on at the end, slowing things down, adding cost, and not really making anything safer?

Good AI governance is not a compliance tax. It's good design. And once you frame it that way, a lot of the apparent tension between "moving fast" and "being responsible" disappears.

# The framework landscape, briefly

A non-exhaustive map of what's out there:

- **ISO 42001** - the first AI management system standard. Lifecycle-oriented. Cares about how the organisation runs, not just what the model does.
- **NIST AI RMF** - risk-based, voluntary, organised around Govern / Map / Measure / Manage. Heavy on practical risk characterisation.
- **IMDA Model AI Governance Framework + AI Verify** - Singapore's framework, with a testing toolkit. Pragmatic and industry-facing.
- **IEEE 7000 series** - ethics-by-design. Starts from stakeholder values and elicits requirements from them.

Each emphasises something different. That's not a weakness. Governance problems differ, so the tools differ.

# There is no one-size-fits-all

One theme that came up was different use cases. A healthcare diagnostic and a marketing recommender have almost nothing in common from a governance perspective. Different risk profiles (someone could die vs. someone might see an annoying ad), different stakeholders (patient, clinician, regulator vs. consumer, advertiser, platform), different regulatory context (medical devices vs. advertising standards), different failure modes.

A single framework stretched over both either over-governs the recommender or under-governs the diagnostic. Usually both, because the checklist gets written for the average case and the average case doesn't exist.

The practical move is to stop treating frameworks as uniforms and start treating them as a toolkit. ISO 42001 gives you the management system. NIST AI RMF gives you the risk vocabulary. IEEE 7000 gives you the stakeholder elicitation method. Sector-specific standards (medical device regulations, financial model risk management, education data privacy) layer on top. Pick the combination that fits the risk, the stakeholders, and the regulatory context you actually have.

# Governance is design, not bolt-on

Almost everything covered by the frameworks - bias testing, explainability, data lineage, auditability, robustness, monitoring, incident response - is cheaper and more effective when designed in from the start than patched on after:
- Bias mitigation after deployment means retraining on new data, re-validating, re-deploying, possibly re-communicating to users. Bias mitigation at dataset construction time means picking better data and testing an evaluation slice. 
- Explainability after the fact means reverse-engineering a black box. Explainability designed in means choosing architectures and logging that make the question answerable in the first place.
- Data lineage as an afterthought is an archaeology project. Data lineage as a design constraint is a few well-placed schemas and a policy.

This isn't new — security-by-design and privacy-by-design made the same argument. AI governance is the same move applied to a new class of system.

# Shift-left

The software-engineering word for this is *shift-left*: move quality activities earlier in the lifecycle, where defects are cheap to fix. It's also the lean argument - a defect caught at design review costs nothing; the same defect caught in production can cost the company.

Concretely for AI:

- **Bias and fairness checks in CI**, not in an annual audit.
- **Model cards and datasheets as build artefacts**, generated automatically, not written the week before launch.
- **Risk assessment at design review**, not as a gate before go-live.
- **Red-teaming during development**, not after an incident.
- **Monitoring and incident response designed alongside the model**, not retrofitted once something goes wrong.

None of this is exotic. It's the same discipline that makes good software engineering work, applied to the parts of AI development that currently get skipped.

# The human is always accountable

A framework's real job is to answer one question clearly: *who is responsible for what, at which stage?* The AI can't be accountable. A model cannot be sued, fired, or called before a committee. A named human always can. Multiple variants of this question came up during the roundtable.

ISO 42001's management-system approach is, at its core, a machine for making this question unambiguous. So is the "Govern" function in NIST AI RMF. So is the stakeholder mapping in IEEE 7000. The frameworks look different but converge on the same point: there needs to be a decision on the responsible person, and accountability is non-negotiable.

This ties directly back to the [Five Hazardous Attitudes](/post/five_hazards/) I wrote about last year - especially Invulnerability ("it won't happen to us") and Anti-authority ("don't tell me"). Clear accountability is the antidote to both. It forces the organisation to decide, in advance, who owns which risks, and who has the authority to say no.

# Sector adaptation in practice

Some examples:

| Sector | Likely core framework | Layered with |
|---|---|---|
| Financial services | NIST AI RMF + ISO 42001 | Model risk management (SR 11-7, MAS FEAT), sectoral AI guidance |
| Healthcare | ISO 42001 | Medical device regulation, clinical validation standards |
| Public sector | IMDA Model AI Governance Framework (Singapore) or equivalent | Public-sector AI guidance, procurement standards |
| Consumer tech | NIST AI RMF | IEEE 7000 (stakeholder elicitation), privacy law (PDPA/GDPR) |


# Conclusion

Good organisations will realise the importance of good design fast. The rest will treat it as a "paperwork" chore and pay the price for it.