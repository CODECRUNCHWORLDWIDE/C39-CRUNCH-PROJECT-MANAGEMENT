# Challenge 1 — Present One Roadmap to Execs and to the Team

**Time:** ~90 minutes. **Difficulty:** Medium. **No single right answer.**

## The scenario

The Sprint 2 re-plan from Lecture 3 / Exercise 3 is finalized: Advanced Permissions slipped 2 sprints, SOC 2 keeps its September 13 date and first claim on remaining capacity, and Onboarding revamp is 2 points over what's left. You now need to actually present this — twice, to two very different audiences, in the same week:

- **Thursday:** a 10-minute slot in Priya's board-prep review. The board doesn't know what a story point is and doesn't need to. They care whether the enterprise renewals close and whether anything else important is at risk.
- **Friday:** Marcus's team planning session. The engineers need to know exactly what's shipping in which sprint, what got cut, and why — because they're the ones who have to execute it starting Monday.

## Your task

Produce **two artifacts** from the same underlying facts (the Exercise 3 re-plan — reuse your own numbers, or the lecture's if you didn't do Exercise 3 first):

1. **`exec-summary.md`** — half a page, maximum. Written for Priya's board slot. Must include: the outcome-level status of Enterprise Trust (on track / at risk / why), explicit confirmation the SOC 2 date holds, one clear sentence about what's at risk elsewhere and why that's an acceptable tradeoff, and nothing below the theme/outcome altitude — no story points, no sprint numbers, no individual backlog item titles.

2. **`delivery-plan.md`** — as detailed as it needs to be. Written for Marcus's team. Must include: every affected item, its new target sprint, the exact scope trimmed or deferred (by name), and the dependency reasoning for why SOC 2 comes first. This is allowed — expected — to be several times longer than the exec summary.

3. **`consistency-check.md`** (short, 100–150 words) — answer directly: could a skeptical outsider read both documents side by side and find zero contradictions? Walk through the one or two places where it would be easiest to accidentally let the two versions diverge (a date stated differently, a risk mentioned in one but not the other) and confirm you didn't.

## Constraints

- Do not invent facts beyond the Exercise 3 re-plan (or the lecture's version of it, if that's your source). If a detail isn't in either, don't state it as fact in either document.
- The exec summary may **not** contain a story point, a sprint number, or an individual backlog item title. If you find yourself needing one to make the summary make sense, that's a sign the sentence belongs in the delivery plan instead — rewrite it at the right altitude.
- The delivery plan must be internally consistent with the exec summary's outcome-level claims — if the exec summary says "Enterprise Trust is on track," the delivery plan cannot show SOC 2 slipping past its date without you flagging that contradiction explicitly and resolving it in `consistency-check.md`.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Altitude discipline | Exec summary leaks sprint numbers or item titles | Exec summary stays strictly at theme/outcome level throughout |
| Honesty under pressure | Exec summary omits the Onboarding risk to look cleaner | Exec summary names the tradeoff plainly, in one sentence, without alarm or euphemism |
| Delivery plan usefulness | Vague ("some things moved around") | An engineer could open it Monday morning and know exactly what to build first |
| Consistency | Not checked, or checked superficially | Specific near-miss points named and resolved, not just asserted "these agree" |

## Submission

Commit all three files to your portfolio under `c39-week-05/challenge-01/`.
