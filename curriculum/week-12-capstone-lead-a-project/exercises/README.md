# Week 12 — Exercises

Three exercises, done in order, directly on your own capstone project — there's no separate practice dataset this week; everything here operates on the real thing you're building and shipping.

## How to work these

- **Exercise 1** happens Monday, right after Lecture 1 — it *is* your charter and board setup, not practice for it. Don't treat it as optional if you already "basically know what you're building"; writing it down is the exercise.
- **Exercise 2** happens Thursday, after you have at least 3–4 real days of logged flow data in `capstone_items`/`capstone_status_history`. Running it earlier than that will produce a technically-correct but nearly meaningless forecast (Lecture 2, §5f) — resist the urge to jump ahead.
- **Exercise 3** happens Friday, before you ship — it produces the actual gate and rollback plan Lecture 3 walks through, not a rehearsal of one.

## What to submit

For each exercise, keep the file(s) it produces in your capstone project's own repo (not the course repo) — `charter.md`, `delivery-approach.md`, a forecast notebook or script, `release-gate.md`, and `ROLLBACK.md`. Your mini-project submission (`mini-project/README.md`) links back to all of them from your capstone repo, so keep everything in one place from Exercise 1 onward.

## Getting unstuck

- If you're not sure your project is scoped small enough for Exercise 1, re-read Lecture 1, §2 before writing anything — the sizing rule there is the exercise's actual prerequisite.
- If Exercise 2's forecast produces a wildly wide range (p50 = 1 day, p95 = 40 days), that's not a bug — it's telling you your throughput history is too thin or too erratic to forecast confidently yet. Say so honestly in your write-up rather than picking a narrower-looking number.
- If Exercise 3's gate fails (a `must` item isn't `Done`), that's the gate working correctly. Don't ship past a failed gate to "finish the exercise" — the exercise is running the gate honestly, not passing it.

| # | Exercise | Skill |
|--:|----------|-------|
| 1 | [exercise-01-capstone-charter.md](./exercise-01-capstone-charter.md) | Charter, backlog, board setup |
| 2 | [exercise-02-capstone-forecast.md](./exercise-02-capstone-forecast.md) | SQL throughput query + Monte Carlo forecast in pandas |
| 3 | [exercise-03-capstone-release-gate.md](./exercise-03-capstone-release-gate.md) | Definition of done, release gate, tested rollback plan |
