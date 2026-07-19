# Exercise 2 — Query Velocity and Cycle Time in SQL

**Goal:** Turn Lecture 2's patterns into your own muscle memory — write the velocity, throughput, WIP, and cycle-time queries yourself, unaided, and check each result against the expected value.

**Estimated time:** 90 minutes.

## Setup

All three tables (`sprints`, `issues`, `issue_status_history`) must be loaded — confirm `SELECT COUNT(*) FROM issue_status_history;` returns `112` before starting (that's Exercise 1). Create a file `solutions.sql` and put each answer under a `-- Task N` comment.

## Tasks

### Task 1 — Velocity per sprint (10 min)

Write the `GROUP BY` query that returns each sprint's velocity (`SUM(story_points)` for `Done` issues).

*Expected:* `Sprint 1: 13, Sprint 2: 18, Sprint 3: 21, Sprint 4: 15, Sprint 5: 17, Sprint 6: 13, Sprint 7: 8`. Confirm your Sprint 7 number, then write one sentence explaining why it needs a caveat the other six don't.

### Task 2 — Velocity trend (10 min)

Using only the closed sprints (1–6), compute the **average velocity** and the **velocity of the most recent closed sprint** side by side, so you can compare "typical" to "most recent" in one query.

*Expected:* average ≈ `16.2`, Sprint 6 = `13`. Sprint 6 is below the six-sprint average — note that down, Challenge 2 asks you to explain it.

### Task 3 — Throughput by week (15 min)

Write the weekly throughput query from Lecture 2 (count of issues resolved per calendar week). Run it and find the single week with the **highest** throughput and the week with the **lowest** (excluding weeks with zero issues resolved — don't count silence as "low throughput," it just means no `resolved_at` fell in that week).

*Expected:* multiple weeks tie for the low end at 1–2 issues; at least one week has 4. Write down which weeks.

### Task 4 — Current WIP, two ways (15 min)

1. Write the simple current-WIP query from `issues.current_status` (Lecture 2, section 5).
2. Write the point-in-time WIP query against `issue_status_history` for **today's actual snapshot date, 2026-04-06** (not a past date this time), and confirm the two queries agree.

*Expected:* both return `3`.

### Task 5 — Cycle time for every completed issue (15 min)

Write the join query from Lecture 2 that returns `issue_key`, `story_points`, and `cycle_time_days` for every `Done` issue, sorted longest cycle time first.

*Expected:* 23 rows. The top three, longest first: `ATLAS-115` (14), `ATLAS-112` and `ATLAS-119` (11, tied).

### Task 6 — Cycle time by issue type (15 min)

Extend Task 5: `GROUP BY issue_type` and compute the **average** cycle time for `story` vs. `bug` vs. `task`. Before running it, predict which type will have the shortest average cycle time and why — then check yourself against the result.

*Expected:* bugs finish fastest by a wide margin. Write one sentence on why that makes sense operationally (hint: think about what "unsized" and "urgent-by-default" usually mean for how a team prioritizes its queue).

### Task 7 — Blocked-time leaderboard (10 min)

Write the `LEFT JOIN` + `GROUP BY` + `HAVING` query from Lecture 2 that returns every issue that spent time `Blocked`, with total days blocked, sorted worst first.

*Expected:* 5 rows (the currently-still-blocked `ATLAS-126` will **not** appear — that's expected, and Lecture 2 explains exactly why). Top row: `ATLAS-119` at 7 days.

## Expected results (spot-check table)

| Task | Key number(s) |
|-----:|----------------|
| 1 | Sprint 3 = 21 (peak), Sprint 6 = 13 (recent low) |
| 2 | 6-sprint average ≈ 16.2, Sprint 6 = 13 |
| 4 | WIP = 3 (both methods agree) |
| 5 | Longest cycle time = `ATLAS-115` at 14 days |
| 6 | Bugs have the shortest average cycle time |
| 7 | `ATLAS-119` blocked longest at 7 days; `ATLAS-126` absent |

## Done when…

- [ ] All 7 queries are in `solutions.sql`, each under a `-- Task N` comment.
- [ ] Every result matches the spot-check table above.
- [ ] You can explain, without looking back at the lecture, why Task 7 doesn't include `ATLAS-126`.
- [ ] You've written the one-sentence explanations asked for in Tasks 1, 3, and 6.

## Stretch

- Write a query that returns, per **assignee**, their count of completed issues and their average cycle time. Is there a meaningful spread across the five engineers, or is it mostly noise given the sample size? (Be honest — 23 completed issues split five ways is a small sample per person; don't over-read it.)
- Write a query that computes lead time (`resolved_at - created_at`) alongside cycle time for every `Done` issue, and find the issue with the **biggest gap** between the two — that gap is exactly how long the issue sat untouched before anyone started it.

## Submission

Commit `solutions.sql` to your portfolio under `c39-week-08/exercise-02/`.
