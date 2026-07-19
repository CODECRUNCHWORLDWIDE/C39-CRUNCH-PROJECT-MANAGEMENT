# Week 8 — Homework

Four problems, ~3 hours total, spread across the week. These reinforce the lectures with a mix of hands-on queries, a written explanation, and a small dataset extension. Commit each.

All queries run against the `sprints` / `issues` / `issue_status_history` seed from the [README](./README.md) unless a problem says otherwise.

---

## Problem 1 — Confirm both engines agree (30 min)

If you only worked in one engine this week, get the other one running too — the point of learning both is trusting neither blindly.

1. Load `sprints`, `issues`, and `issue_status_history` into **both** PostgreSQL and SQLite.
2. Run `SELECT COUNT(*) FROM issue_status_history;` in each — both must return `112`.
3. Run the current-WIP query (Lecture 2, Section 5) in each — both must return `3`.
4. Run the velocity-by-sprint query in each and confirm the seven rows are byte-for-byte identical between engines.

**Deliver** `setup.md` with: confirmation all four checks passed on both engines, and one sentence on any syntax you had to change between them (there should be very little — this week's core queries were written to be portable).

---

## Problem 2 — Twenty warm-up queries (90 min)

Write and run each. Put them in `warmups.sql` with a `-- N` comment and the result beneath each as a comment.

1. All issues in `Sprint 3`, sorted by `story_points` descending.
2. Every `bug`, regardless of sprint, sorted by `created_at`.
3. Every issue assigned to Nadia Petrova, with `issue_type` and `current_status`.
4. The three issues with the highest `story_points` among all `Done` issues.
5. Every issue that has never been `Blocked` at any point (hint: a `NOT IN` or `NOT EXISTS` against `issue_status_history`).
6. Every issue currently sitting in `To Do` or `Backlog` (i.e., not yet started).
7. The total number of distinct `status` values that appear anywhere in `issue_status_history`.
8. For each `priority` level, the count of issues at that priority.
9. Every issue with `issue_type = 'task'`.
10. The single issue with the shortest cycle time among all `Done` issues (a one-row answer, not the full sorted list).
11. Every `Blocked` interval (any issue) that lasted **more than 3 days** — compute the duration in the query itself, don't eyeball it.
12. The sprint with the highest velocity, and its value, in one query (not "look at the table and pick the biggest number").
13. Every issue whose `assignee` is Wei Zhang **and** whose `current_status` is `Done`.
14. The `sprints` row for the sprint that is currently open (hint: `CURRENT_DATE BETWEEN start_date AND end_date` — but remember `CURRENT_DATE` on your machine won't be 2026-04-06, so also write the hardcoded-date version and compare).
15. Every issue created **before** its sprint's `start_date` (i.e., groomed ahead of time rather than added mid-sprint) — a join between `issues` and `sprints`.
16. Every issue created **on or after** its sprint's `start_date` (the opposite of #15 — these are the mid-sprint scope additions; there should be exactly 2).
17. The average `story_points` per `issue_type`, ignoring `NULL`s (recall: `AVG` already ignores `NULL` on its own).
18. Every issue where `resolved_at` fell in a **different sprint** than the sprint recorded in `issues.sprint` would suggest, if you naively assumed resolution always happens within the labeled sprint's date range — i.e., issues resolved after their sprint's `end_date`. *(There should be a small number — these are your spillover/carryover issues.)*
19. The full name list of every distinct `assignee` in `issues`, alphabetically.
20. A single query returning, per sprint, both its velocity **and** its count of `Done` issues side by side (so you can eyeball whether high velocity always means high issue count, or whether they can diverge).

---

## Problem 3 — Explain the metrics, in prose (30 min)

In `metrics-writeup.md`, answer in prose (no more than 400 words total):

1. Run Problem 2's #18 query. How many issues resolved after their labeled sprint's `end_date`? Pick one and explain, in your own words, why its `sprint` field still says the sprint it was *committed* to rather than the one it finished in — and why that convention is exactly what makes raw velocity numbers slightly untrustworthy without also checking throughput.
2. Explain, without notes, the difference between cycle time and lead time — then say which one you'd compute if a stakeholder asked "how long does it typically take from when someone files a request until we close it out."
3. In one paragraph, explain Little's Law and apply it to a made-up example that is **not** Atlas: pick any two of (WIP, throughput, cycle time), invent plausible numbers, and solve for the third.
4. Name one real way a team could inflate its velocity number without doing more real work, and one real way it could inflate throughput instead. They should be different tricks.

---

## Problem 4 — Extend the dataset with a planned Sprint 8 (60 min)

Sprint 8 hasn't started yet, but Atlas already has a rough backlog for it. Make the data your own.

1. Insert a new `sprints` row for `'Sprint 8'` (pick reasonable `start_date`/`end_date` two weeks after Sprint 7 ends, and a one-line `goal`).
2. Write `INSERT` statements for **4 new issues** into `issues`, all with `sprint = 'Sprint 8'` and `current_status = 'Backlog'` (nothing has started yet) — invent believable `summary` text that continues the Atlas story, and give at least one of them a `NULL` `story_points` (an unsized bug or spike).
3. Confirm your new rows don't break anything: `SELECT COUNT(*) FROM issues;` should now read `32`, and the velocity-by-sprint query from Lecture 2 should show `Sprint 8` with velocity `0` (or absent, depending on how you wrote the `WHERE current_status = 'Done'` filter — if it's absent, that's worth a sentence on why).
4. Then **undo** it cleanly: `DELETE FROM issues WHERE sprint = 'Sprint 8'; DELETE FROM sprints WHERE sprint_name = 'Sprint 8';` and confirm you're back to 28 issues, 7 sprints.

**Deliver** `extend.sql` (the inserts, the two confirmation queries with their output, and the cleanup) plus one sentence on what you noticed about how a freshly-planned sprint looks in every query compared to a closed one.

---

## Time budget

| Problem | Time |
|--------:|----:|
| 1 | 30 min |
| 2 | 90 min |
| 3 | 30 min |
| 4 | 60 min |
| **Total** | **~3.5 h** |

After homework, take the [quiz](./quiz.md) and ship the [mini-project](./mini-project/README.md).
