# Week 4 — Homework

Five problems, ~4 hours total, spread across the week. These reinforce the lectures with a mix of hands-on SQL/Python, a written explanation, and a build-your-own-scenario task. Commit each.

All queries run against the Week 4 seed tables from the [README](./README.md) unless a problem says otherwise.

---

## Problem 1 — Build your own reference set (45 min)

Reference-class forecasting only works if you have a real reference class. Practice building one from scratch.

1. Pick any three tasks from **your own life** in the last month that took noticeably longer than you expected (a school assignment, a home project, a move, anything with a real "planned" and "actual" duration you can recall).
2. Estimate, honestly, what you thought each would take *before* you started, and how long each actually took.
3. Compute your own personal overrun factor, the same way Lecture 2 computed Atlas's: `total actual / total planned`.

**Deliver** `personal-reference-class.md`: the three tasks, your planned/actual numbers, your computed factor, and two sentences on whether it's closer to Atlas's 1.32 or the outside reference class's 1.40 — and what that tells you about your own planning fallacy.

---

## Problem 2 — Extend `atlas_story_history` (60 min)

Sprint 2 wasn't the last word — invent Sprint 3's actual outcome (once it's run, hypothetically) and extend the history.

1. Using your own Exercise 1 point values for whichever items you'd have committed to Sprint 3 (Challenge 2's plan), write `INSERT` statements adding those stories to `atlas_story_history` with invented-but-plausible `actual_days` values.
2. Recompute the overall `overrun_factor` across all three sprints now (`SUM(actual_days) / SUM(ideal_days_est)`).
3. Did adding Sprint 3 move the factor closer to `1.0` (team getting better at estimating) or further from it? By how much?

**Deliver** `extend-history.sql` (the inserts and the recomputed query) plus two sentences on what you'd conclude about Atlas's estimating skill after three sprints of real data, versus after just Sprint 1's two data points.

---

## Problem 3 — Explain commitment vs. forecast to a non-PM (45 min)

In `explain-commitment.md`, write **exactly 150–200 words**, in plain English, explaining the difference between a sprint commitment and a sprint forecast to someone who has never worked in Agile — imagine a friend outside tech, not a colleague. No jargon like "story points" or "velocity" without defining it in the same sentence. Use a non-software analogy if it helps (a moving-day estimate, a road-trip ETA — anything with the same "best case vs. realistic case" structure).

---

## Problem 4 — Ten warm-up capacity calculations (60 min)

Write and run each in `capacity-warmups.sql`, with a `-- N` comment and the resulting number beneath each. Assume an 8-hour day unless stated.

1. A person with `working_days=10, leave_days=2, meeting_hours_per_day=1.0, focus_factor=0.75` — their `focused_days`.
2. Same person, but `meeting_hours_per_day` rises to `3.0` — how much `focused_days` drops.
3. A 3-person team, each with `working_days=10, leave_days=0, meeting_hours_per_day=1.0, focus_factor=0.8` — team `total_focused_days`.
4. That same 3-person team's capacity in points, using Atlas's `1.32` actual pace.
5. That same team's capacity in points, using the `1.00` ideal pace (the stretch number).
6. The gap, in points, between #4 and #5 — this is the team's uncertainty band in one number.
7. If one of the 3 people goes on 5 days of leave (half the sprint), recompute `total_focused_days`.
8. The percentage drop in capacity that single person's leave caused.
9. A team of 5 with a lower average `focus_factor` of `0.6` (heavy interrupt load) vs. a team of 4 with `focus_factor` of `0.85` (protected focus time) — same `working_days=10, leave_days=0, meeting_hours_per_day=1.0` for everyone. Which team has higher `total_focused_days`? By how much?
10. Using #9's result: what does this tell you about whether "add another person" or "protect the team's existing focus time" is the more reliable lever for raising capacity?

---

## Problem 5 — Design a recalibration checklist (50 min)

Building on Challenge 1's Team Beacon scenario, write a **reusable checklist** — not specific to Beacon — that any team could run quarterly to catch point inflation before it goes six sprints unnoticed. In `recalibration-checklist.md`:

1. At least 5 concrete checks (e.g., "pull 3 recently-completed stories at each point value and re-run planning poker on them blind — does the team still agree on the size?").
2. For each check, one sentence on what a *failing* result would look like.
3. A one-paragraph recommendation for how often to run it, and why that cadence (not too disruptive, not too infrequent to catch drift early).

---

## Time budget

| Problem | Time |
|--------:|----:|
| 1 | 45 min |
| 2 | 60 min |
| 3 | 45 min |
| 4 | 60 min |
| 5 | 50 min |
| **Total** | **~4.3 h** |

After homework, take the [quiz](./quiz.md) and ship the [mini-project](./mini-project/README.md).
