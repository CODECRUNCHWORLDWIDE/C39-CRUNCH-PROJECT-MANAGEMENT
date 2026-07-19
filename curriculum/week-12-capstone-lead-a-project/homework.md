# Week 12 — Homework

Extra practice, spread across the week, tying the whole course together. Unlike most weeks, this homework doesn't introduce new material — it drills the integrative judgment calls that show up when every skill from Weeks 1–11 has to work at once, on something real. Do these alongside the mini-project, not instead of working on it.

---

## H1 — Reconstruct a charter from a slipping project (Monday, ~30 min)

You inherit this incomplete status update, sent to you cold, no other context:

> "So the login thing's mostly done I think, except Dana wants social login too now, which nobody scoped originally. We're supposed to ship end of week but honestly IDK, depends on the OAuth provider's docs which are a mess. Sarah said it's fine to slip if we need to."

Write, from this alone: (a) three specific questions you'd need answered before this could become a real charter, (b) what's missing from it that Week 1's charter anatomy requires, (c) one sentence naming the scope-creep risk sitting inside "Dana wants social login too now" and how you'd handle it per Week 1's triple-constraint rule (make the trade-off explicit).

## H2 — Little's Law sanity check (Tuesday, ~20 min)

A friend tells you: "My team's WIP is 12, our throughput is 3 items/week, and our average cycle time is 2 weeks." Using Little's Law (Week 8), check whether these three numbers are internally consistent. They aren't — show the arithmetic, and write one sentence on what a team should investigate when its own numbers stop agreeing with Little's Law (Week 8, §6).

## H3 — Design a WIP-limit-of-1 test (Wednesday, ~25 min)

For your own capstone (or Jordan's TaskPing, if yours hasn't produced enough data yet), write a SQL query against `capstone_items`/`capstone_status_history` that would tell you, after the fact, **how many times you broke your own WIP limit of 1** — i.e., how many days had more than one item with `current_status = 'In Progress'` simultaneously. Run it against your real data if you have any yet.

```sql
-- starter shape — you'll need to reason about entered_at/left_at overlaps
SELECT entered_at, COUNT(*) AS items_in_progress_that_day
FROM capstone_status_history
WHERE status = 'In Progress'
GROUP BY entered_at
HAVING COUNT(*) > 1;
```

## H4 — Forecast under a thin history (Thursday, ~30 min)

Run the Monte Carlo simulation from Lecture 2, §5e using this deliberately tiny, three-day throughput history: `[3, 0, 1]`, with 5 items remaining. Report p50/p85/p95. Then run it again with the same mean but a longer, more realistic ten-day history: `[3, 0, 1, 2, 1, 0, 2, 1, 3, 1]`, same 5 items remaining. Compare the two p85 values and write two sentences on why the longer history produces a different (almost certainly tighter, more trustworthy) answer even though both have roughly the same average.

## H5 — Write a rollback plan for something that isn't code (Friday, ~20 min)

Release gates and rollback plans (Week 10, Lecture 3 this week) are usually taught with code in mind. Pick one non-code "release" from your own life or work — publishing a blog post, sending a mass email, changing a pricing page, updating a public document — and write a 5-line rollback plan for it: what "known-good state" means when there's no git tag, and what the literal first three actions would be if it needed reverting within the hour.

## H6 — Retrospective on the course itself (Saturday, ~40 min)

Run Week 2's retrospective format (what went well / what didn't / what you'd change / one data point) — not on your capstone this time, but on **the whole 12-week course**. For the "one data point" slot, use something concrete and countable: how many exercises you actually completed, how many quiz questions you missed across all 12 weeks if you tracked them, or the total hours the weekly schedules estimated versus what you actually spent. Keep this one for yourself — it's not graded, but it's the same discipline Week 12 asked you to apply to a project, applied one level up to your own learning.

---

## Self-check

- H1–H2 test whether Weeks 1 and 8's core models (the charter anatomy, Little's Law) are still automatic, not just passable on last week's quiz.
- H3 tests whether you can turn a policy ("WIP limit of 1") into a real SQL check instead of trusting yourself to remember to follow it.
- H4 is the single most important homework item this week — if the two forecasts don't clearly differ in trustworthiness to you, re-read Lecture 2, §5f before starting the mini-project's forecast for real.
- H5 checks whether "release gate and rollback" generalized in your head beyond "a thing programmers do," which is the actual test of whether you understood the concept or just the syntax.
