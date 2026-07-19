# Challenge 2 — Choose a Delivery Approach

**Time:** ~60 minutes. **Difficulty:** Medium. **No single right answer — but a wrong one is easy to write.**

## The scenario

You're the PM being consulted on three unrelated briefs across Northlight and one of its enterprise customers. For each, you need to recommend **predictive, iterative, or Agile** (Lecture 3, §4) and be ready to defend it to a skeptical tech lead who will push back on a lazy answer.

## Your task

For each brief below, in `approach-choices.md`:

1. **State your recommended approach** (predictive, iterative, or Agile).
2. **Defend it in 3–5 sentences**, citing the *specific* facts in the brief that drove your choice — not a generic definition restated. Reference Lecture 3, §4's "fits when / doesn't fit when" criteria explicitly.
3. **Name the strongest argument for a different approach**, and explain in 1–2 sentences why you rejected it anyway. (If you can't think of a counter-argument, you probably haven't thought hard enough — every real choice here has a genuine trade-off.)

## The three briefs

### Brief A — Regulatory compliance report

Northlight is legally required to submit a specific, government-defined quarterly data report to a financial regulator, starting in 4 months. The report's exact required fields, format, and submission schedule are published in a regulatory document and will not change before the deadline (regulatory formats change, but on a multi-year cycle, not mid-project). The regulator will reject and penalize the company for a late or malformed submission — there is no partial credit for "mostly working." Failure has real financial and legal consequences, not just an annoyed stakeholder.

### Brief B — New AI-powered "smart dashboard suggestions" feature

Priya wants to explore whether Northlight Insights can suggest useful dashboard configurations to customers automatically, based on their data. Nobody at Northlight has built a recommendation-style feature before. The team has hypotheses about what customers might find valuable, but genuinely doesn't know which of several possible approaches (rule-based suggestions vs. a simple ML model vs. showing "customers like you also built…") will actually resonate, or which data signals matter most, until real customers interact with something. Priya explicitly says she wants to "learn fast and be willing to throw away the first version if it's wrong."

### Brief C — Migrating Northlight's authentication system to a new identity provider

Northlight is switching from its current login/auth provider to a new one across the entire product, because the current provider is being discontinued by its vendor in 6 months (a hard external deadline, not chosen by Northlight). The migration path is well understood — the new provider publishes a clear migration guide, the team has done similar migrations before, and the main risk is careful sequencing and thorough testing of a known, bounded scope (user accounts, SSO configs, session handling), not discovering new requirements along the way.

## Constraints

- Don't default to "Agile" for every brief because it's the trendy answer — Lecture 3 is explicit that Agile is a poor fit for some real situations, and this challenge is specifically testing whether you can tell the difference.
- Your defense must cite *specific facts from the brief*, not the general definition of the approach you picked. "Agile fits because it's flexible" is not a defense — it's a definition. Explain why *this brief's* facts make flexibility the right call (or don't).

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Recommendation | Same approach for all three, or picks the "safe-sounding" one without reasoning | Distinct, brief-specific reasoning for each; not all three land on the same approach |
| Defense | Restates the approach's textbook definition | Cites specific facts from the brief (the regulator's hard deadline, the team's genuine uncertainty in Brief B, the well-understood scope in Brief C) |
| Counter-argument | Skipped, or a strawman | A real, honestly-stated trade-off, with a clear reason it was outweighed |

## Submission

Commit `approach-choices.md` to your portfolio under `c39-week-01/challenge-02/`.
