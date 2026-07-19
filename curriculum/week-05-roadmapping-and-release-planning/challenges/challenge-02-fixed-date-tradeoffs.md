# Challenge 2 — Cut a Release Plan to Hit a Fixed Date

**Time:** ~90 minutes. **Difficulty:** Hard. **No single right answer.**

## The scenario

New information, independent of the Lecture 3 slip: Northlight's SOC 2 auditor just informed Elena that their **fieldwork window has moved up three weeks**, from September 14 to **August 24**. The reason is outside Northlight's control (the auditor had a cancellation and offered the earlier slot, and the alternative was waiting until January) — Priya has accepted the new date and it is not negotiable a second time.

This means audit logging & access reviews (item 3, 20 points, `depends_on` item 2, Advanced Permissions) must be **live and stable by August 23** — inside **Sprint 2** (2026-08-17 to 2026-08-30), not Sprint 3 as originally planned. Use the **original, un-slipped** release plan from Lecture 2 as your starting point (i.e., assume the Lecture 3 slip has *not* happened — this is a separate, independent scenario).

Original Sprint 1–2 capacity: 19 + 19 = 38 points. Original Sprint 1–2 plan: item 1 (8 pts) + item 2, Advanced Permissions (30 pts) = 38, exact — with **zero room left over**, and SOC 2 (20 pts) wasn't even in that window originally.

## Your task

Produce `fixed-date-tradeoffs.md` containing:

1. **State the real conflict, with numbers.** Sprints 1–2 have 38 points of capacity. The new requirement is that items 1, 2, **and** 3 (8 + 30 + 20 = 58 points) must all complete inside Sprints 1–2. Quantify the gap precisely (how many points over capacity) before proposing anything — a vague "this is really tight" is not an acceptable start.

2. **Identify every lever available**, per Week 1's triple constraint (scope / time / cost) applied here at release grain, and evaluate each honestly — including the ones you'll reject:
   - **Cut scope** — what, specifically, could be trimmed from items 1–3 to close a 20-point gap? (Be specific: name a piece of Advanced Permissions or the launch hardening you'd defer, not "cut 20 points" in the abstract.)
   - **Add capacity** — could Marcus's team temporarily add people or hours? Evaluate honestly using what Lecture 2, §6 and Week 1's triple-constraint lecture say about this lever's real cost (onboarding drag, and whether it can even land inside two weeks).
   - **Move the date** — already established as fixed and non-negotiable per the scenario. State explicitly why you're not choosing this lever, rather than silently skipping it.
   - **Descope something else on the roadmap entirely** — could a Now/Next item be pushed to Later to free capacity, even if it's not one of items 1–3? (Hint: consider whether Data Export API or Onboarding revamp, originally scheduled for R2/R3, could absorb a schedule shift instead, freeing Marcus's team to concentrate on 1–3 first.)

3. **Recommend a specific combination of levers** that closes the 20-point gap, with real numbers reconciling to 38 (or state, if you conclude it's genuinely impossible within Sprints 1–2 alone, what you'd actually recommend instead — e.g., borrowing capacity from Sprint 3 and accepting item 3 finishes August 30 with 6 days of schedule risk against the August 23 target, and what you'd do about *that* gap).

4. **Write the tradeoff conversation** you'd have with Priya and Elena — 150–200 words — presenting your recommendation as an honest choice they're making with you, not a decision you've already made unilaterally and are just informing them of.

## Constraints

- You may not simply declare the auditor's date "actually flexible" — the scenario states explicitly that it isn't, a second time.
- Every scope cut must name the specific piece of work removed, not just a point count.
- If your final recommendation still doesn't reconcile to real numbers (points removed + points added + points shifted elsewhere actually closes the gap), the answer is incomplete regardless of how reasonable the prose sounds.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Problem framing | Skips straight to a fix without quantifying the 20-point gap | States the gap precisely before proposing anything |
| Lever evaluation | Only considers cutting scope | Evaluates all four levers honestly, including why "add capacity" and "move the date" are rejected |
| Recommendation | Vague ("we'll figure out what to cut") | Names specific pieces of work, with numbers that reconcile |
| Stakeholder conversation | One-sided announcement | A real, respectful choice framed for Priya and Elena to weigh in on |

## Submission

Commit `fixed-date-tradeoffs.md` to your portfolio under `c39-week-05/challenge-02/`.
