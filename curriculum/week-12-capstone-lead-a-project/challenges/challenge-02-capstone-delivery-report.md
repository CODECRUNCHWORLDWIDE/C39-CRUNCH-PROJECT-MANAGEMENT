# Challenge 2 — Write a Data-Backed Final Delivery Report

**Skill:** Lecture 3, §5, taken further than the mini-project requires — a complete, publication-quality delivery report built entirely from your own capstone's real data, written to withstand a skeptical reader who wasn't in the room all week.

**Estimated time:** 1.5 hours. Do this after you've shipped (Exercise 3) and run your retrospective (Lecture 3, §4).

---

## Why this is harder than it sounds

Lecture 3, §5 gives you the report's skeleton. Most first attempts at filling it in drift into one of two failure modes: **marketing** (only the wins, vague on the misses) or **diary** (an honest but unstructured play-by-play with no numbers a stranger could verify). This challenge asks for the third thing, which is genuinely harder to write: a report that is both **completely honest** and **completely load-bearing** — every claim backed by a query you can show, every miss stated as plainly as every win.

## The test

Before you start writing, write down the name of one real person who was *not* involved in your capstone at all — a friend, a classmate, anyone who knows nothing about what you built this week. You're writing this report **for them**, cold. If a sentence in your report only makes sense to someone who watched you build it, rewrite it.

## Requirements

Your `delivery-report.md` must include, each backed by an actual query or number you can point to (not a recollection):

1. **What shipped** — the release tag, one plain-language paragraph, and a link to the repo.
2. **Every charter success criterion**, each with:
   - The exact SQL query or computation that checks it.
   - The actual result.
   - Met / partially met / missed — stated plainly, not softened.
3. **A comparison of your Lecture 2/Exercise 2 forecast against what actually happened** — the p50/p85/p95 you predicted on Thursday, the actual ship date, and the gap in days, with a one-sentence explanation of the gap grounded in your risk register (not "stuff came up").
4. **A cycle-time breakdown** for your three longest-running items — compute each one's cycle time (Week 8's definition: `resolved_at - first_in_progress_at`) directly from `capstone_status_history`, and name, for each, the specific reason it took as long as it did.
5. **A scope-change log** — every item that moved between in-scope and out-of-scope after the charter was written, with the date and reason for each change.
6. **One retrospective takeaway**, verbatim from Lecture 3, §4's retrospective, that materially connects to something in sections 2–5 above (not a throwaway line that doesn't tie back to the data).

## Part A — Draft (50 min)

Write the report in full, following the six requirements above in order. Use real query output, not paraphrased numbers — paste the SQL and its result for every claim in sections 2–4.

## Part B — Stress-test it (25 min)

Give the draft to the "cold reader" test from above — literally send it to that person if you can, or read it once yourself pretending you've never seen the project. For each of the following, either fix the report or write a one-line note explaining why it's fine as-is:

- Is there any sentence a stranger couldn't verify from the numbers given?
- Is there any missed success criterion described in softer language than a met one?
- Does the forecast-vs-actual comparison in requirement 3 actually explain the gap, or just restate that there was one?
- Could someone reconstruct your project's actual timeline from this report alone, without having watched you build it?

## Part C — Finalize (15 min)

Revise based on Part B, and add a one-paragraph **executive summary** at the very top — 3-4 sentences a busy reader could stop after and still know what happened, written last, after you know exactly what the report says.

---

## Expected outcome

A `delivery-report.md` that could be handed to a real stakeholder — or a future employer asking "tell me about a project you shipped end-to-end" — with every factual claim traceable to a query you ran against your own `capstone_pm` database.

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|---------------------|
| Every claim is data-backed | 30% | Each of the 6 requirements has a real, pasted query/result, not a paraphrase |
| Honesty about misses | 25% | Missed or partial criteria are stated as plainly as met ones, with real cause named |
| Forecast-vs-actual analysis | 20% | The gap is explained with a specific, risk-register-grounded cause, not hand-waved |
| Cold-reader clarity | 15% | A stranger could follow the whole report without prior context |
| Executive summary | 10% | Accurate, complete in 3-4 sentences, written last |

## Stretch goal

Turn `delivery-report.md` into a one-page rendered artifact (HTML or a short slide) you could actually show someone in 60 seconds — the report's content should already support this; the stretch is presentation, not new analysis.
