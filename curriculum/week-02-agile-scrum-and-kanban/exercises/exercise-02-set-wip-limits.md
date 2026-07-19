# Exercise 2 — Set WIP Limits on a Flooded Board

**Goal:** Stop eyeballing a board and start setting a WIP limit you can defend with a query and a formula — using Lecture 3's age and WIP metrics against a deliberately flooded `in_review` column.

**Estimated time:** 45–60 minutes.

## Setup

Make sure `atlas_backlog` is seeded (see the [README setup](../README.md#set-up-this-weeks-data-do-this-first)). Re-read Lecture 3, §2 (why WIP limits are the whole point) and §5a–b (WIP and age queries) before starting. Create a file `wip-limits.md` in your portfolio at `c39-week-02/exercise-02/`.

## Scenario

It's now a few days later in sprint 2. Code review has quietly become a bottleneck — engineers keep starting new stories rather than reviewing each other's finished work, because starting something new *feels* more productive than reviewing, even though nothing is actually shipping faster. Run the following against `atlas_backlog` to simulate the flood that's built up:

```sql
INSERT INTO atlas_backlog VALUES
(21,'Comment permalink deep link','story',2,21,'in_review',2,'Sam O.','2026-01-19','2026-01-24',NULL),
(22,'Comment edit history','story',3,22,'in_review',2,'Dana K.','2026-01-19','2026-01-26',NULL),
(23,'Notification digest email','story',3,23,'in_review',2,'Wes T.','2026-01-19','2026-01-27',NULL),
(24,'Comment markdown rendering','story',2,24,'in_review',2,'Sam O.','2026-01-26','2026-01-29',NULL),
(25,'Fix: comment timestamp timezone bug','bug',1,25,'in_review',2,'Dana K.','2026-01-28','2026-01-30',NULL),
(26,'Comment reaction emoji','story',3,26,'in_review',2,NULL,'2026-01-28','2026-01-31',NULL);
```

Treat **2026-02-03** as "today" for every age calculation in this exercise (the same reference date Lecture 3 uses), so your numbers are reproducible and checkable against the expected results below.

## Tasks

1. **Query current WIP per column** (Lecture 3, §5a). Report the count for every active status.
2. **Query age in column for every `in_review` item** (Lecture 3, §5b, pinned to `2026-02-03`), sorted oldest first. Report the full result set.
3. **Compute two summary numbers from your Task 2 result:** the average age of an `in_review` item, and the oldest age on the board. Do this with a single SQL aggregate query (`AVG()` and `MAX()`), not by hand-averaging the printed rows.
4. **Apply the capacity rule from Lecture 3, §2** ("roughly one to two items per person actively working that stage, then tune down") to `in_review`. The four engineers (Dana, Sam, Wes, and one more) each split attention between writing new code and reviewing teammates' work — review is real but *secondary* attention for each of them, not their primary job. Using that reasoning, propose a specific numeric WIP limit for `in_review` and justify it in 2–3 sentences, referencing your Task 1 and Task 3 numbers.
5. **Write the policy, not just the number.** One sentence stating exactly what the team does once `in_review` is at its limit (who does what instead of starting new work).

## Expected result (spot checks)

- Task 1: `in_review` should now show **8** items (up from Lecture 3's baseline of 2) — the flood.
- Task 2: the oldest `in_review` item should be **item 21**, at **10 days** old (`2026-02-03` − `2026-01-24`).
- Task 3: average age across all 8 `in_review` items should be **≈4.9 days** ((10+8+7+5+4+3+1+1)/8 = 4.875).
- Task 4: a defensible answer lands **well below 8** — something in the 3–5 range, given 4 engineers with review as secondary attention. If your number is 6 or higher, re-read Lecture 3 §2 — you likely applied the "primary work" multiplier to a "secondary work" column.

## Done when…

- [ ] `wip-limits.md` contains both queries, their full result sets, and the two summary numbers from Task 3.
- [ ] Your proposed WIP limit is a specific integer with a written justification that cites the numbers you actually queried — not a round number pulled from nowhere.
- [ ] The policy sentence names a concrete action ("the next engineer with capacity reviews the oldest item in the column before pulling new work"), not a vague aspiration.

## Stretch

Item 21 (the oldest, at 10 days) turns out to be blocked on a design question, not just waiting for a reviewer's attention — nobody had noticed because "in_review" doesn't distinguish "waiting for a reviewer" from "blocked on someone else." Propose a schema change to `atlas_backlog` (a new column, or a new allowed `status` value) that would let a future query tell these two cases apart, and write the `ALTER TABLE` (or equivalent) statement.

## Submission

Commit `wip-limits.md` to your portfolio under `c39-week-02/exercise-02/`.
