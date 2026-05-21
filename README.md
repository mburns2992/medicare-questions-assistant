# Medicare Questions Assistant — Prototype

A prototype Q&A assistant that answers the ten questions people most commonly ask when first shopping for Medicare coverage. Built as a personal exercise in **scoping a member-facing process**, not just a chatbot demo.

**Live demo:** [mburns2992.github.io/medicare-questions-assistant](https://mburns2992.github.io/medicare-questions-assistant/)

> **Not affiliated with Medicare, CMS, or any insurer.** This is a personal demonstration project built on publicly available Medicare information. Medicare program amounts change every year; answers reflect 2026 figures from CMS / Medicare.gov releases. Always verify at [Medicare.gov](https://www.medicare.gov) or 1-800-MEDICARE.

---

## Why I built it this way

The interesting part of a tool like this isn't wiring up a chat box — it's the process design around it. A wrong Medicare answer (an enrollment window, a penalty) has a real cost to the person asking. So this prototype is built around four deliberate decisions a process owner would make:

1. **A defined intake** — a fixed set of recognized questions, plus a free-text field.
2. **A vetted knowledge base** — every answer is pre-written, human-reviewed, and tied to a published source. Answers are *not* generated live by an AI model.
3. **A control for uncertainty** — when an incoming question doesn't clear a confidence threshold, the assistant escalates to a licensed human instead of guessing.
4. **Measurable outcomes** — the design names the KPIs that would tell you whether the tool is working.

## Process flow

```
        ┌─────────────────────┐
        │  Member question    │
        │  (chip or free text)│
        └──────────┬──────────┘
                   │
                   ▼
        ┌─────────────────────┐
        │  Intake & keyword    │
        │  match to knowledge  │
        │  base                │
        └──────────┬──────────┘
                   │
        ┌──────────┴───────────┐
        │  Confidence check    │
        │  (CONTROL POINT)     │
        └──────┬───────┬───────┘
       clears  │       │  below threshold
   threshold   │       │
               ▼       ▼
   ┌───────────────┐  ┌──────────────────────┐
   │ Vetted answer │  │ Escalate to licensed │
   │ + source cite │  │ human / 1-800-       │
   │               │  │ MEDICARE             │
   └───────┬───────┘  └──────────┬───────────┘
           │                     │
           └─────────┬───────────┘
                     ▼
        ┌─────────────────────┐
        │  Log outcome for     │
        │  KPI measurement     │
        └─────────────────────┘
```

## Controls

| Control | What it prevents | How it works |
|---|---|---|
| Vetted knowledge base | Inaccurate or invented answers | Answers are pre-written and human-reviewed, never free-generated |
| Source citation on every answer | Unverifiable claims | Each answer names its published source (CMS / Medicare.gov) |
| Confidence threshold | Wrong answers to out-of-scope questions | Unmatched questions escalate instead of returning a low-quality guess |
| Annual content review | Stale figures after yearly CMS updates | Knowledge base is dated and re-verified each plan year |
| Visible disclaimer | Misuse as official guidance | Clear non-affiliation notice and a directive to verify with Medicare.gov |

## KPIs

If this moved beyond a prototype, these are the measures that would show whether it's working:

- **Deflection rate** — share of questions fully resolved without a human.
- **Answer accuracy** — answers spot-checked against current CMS source material.
- **Escalation rate** — share routed to a licensed agent (a healthy number, not zero — it's the safety control doing its job).
- **Coverage gaps** — recurring unmatched questions, used to prioritize new knowledge-base entries.

## Content governance

The knowledge base needs an owner. Each plan year, CMS publishes updated premium, deductible, and penalty figures (typically in the fall). The maintenance process:

1. Review CMS / Medicare.gov releases when published.
2. Update affected answers and re-date the knowledge base.
3. Spot-check a sample of answers against current sources.
4. Review unmatched-question logs and add high-frequency gaps.

## Tech

Single static `index.html` — HTML, CSS, vanilla JavaScript. No build step, no dependencies, no API keys. Runs anywhere static files are served, including GitHub Pages.

## Roadmap

- Semantic matching to better catch reworded questions
- Logging hooks for the KPIs above
- Knowledge-base entries split into a separate data file for non-technical editing
- Spanish-language answer set

---

*Personal project. Built independently using publicly available information; not affiliated with or representing any employer.*
