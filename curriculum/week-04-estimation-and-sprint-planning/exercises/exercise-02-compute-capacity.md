# Exercise 2 — Compute a Sprint's True Capacity

**Goal:** Turn `team_capacity_calendar` into a real, defensible Sprint 3 capacity number in points, following Lecture 3's formula exactly — by hand for one person, then in SQL for the whole team.

**Estimated time:** 45–60 minutes.

## Setup

```sql
SELECT COUNT(*) FROM team_capacity_calendar WHERE sprint_id = 3;   -- must print 5
```

Look at the raw roster before computing anything:

```sql
SELECT * FROM team_capacity_calendar WHERE sprint_id = 3 ORDER BY person;
```

Create `exercise-02.sql` for your queries and `exercise-02-notes.md` for your written answers.

## Part A — By hand, one person (10 min)

Before writing SQL, work through the four-step formula from Lecture 3 **by hand** for **Wes T.** (`working_days=10, leave_days=0, meeting_hours_per_day=1.5, focus_factor=0.7`). Show each intermediate number in `exercise-02-notes.md`:

```
available_days = ?
meeting_days   = ?
remaining_days = ?
focused_days   = ?
```

Check your answer against Lecture 3's worked table (Wes's `focused_days` should come out to `5.69`) before moving on — if it doesn't match, find your arithmetic error now, not after you've built the query on top of it.

## Part B — In SQL, the whole team (20 min)

Write one query that computes `available_days`, `meeting_days`, and `focused_days` for **all five** people in one result set, matching Lecture 3's table exactly. Then write a second query that sums `focused_days` into a single `total_focused_days` for Sprint 3.

```sql
-- Task 1: per-person breakdown
SELECT
    person,
    -- your formula here
FROM team_capacity_calendar
WHERE sprint_id = 3;

-- Task 2: team total
SELECT SUM(...) AS total_focused_days
FROM team_capacity_calendar
WHERE sprint_id = 3;
```

*Expected: `total_focused_days ≈ 28.4`.*

## Part C — Convert to points, both ways (15 min)

Pull Atlas's actual historical pace from `atlas_story_history` (don't hardcode `1.32` — derive it):

```sql
SELECT ROUND(SUM(actual_days) / SUM(ideal_days_est), 2) AS actual_pace_days_per_point
FROM atlas_story_history;
```

Now compute **both** conversions of your `total_focused_days` from Part B:

```sql
-- Commitment: focused days / actual historical pace
SELECT ROUND(28.4 / 1.32, 1) AS commitment_points;   -- replace with your real total_focused_days and derived pace

-- Stretch: focused days / ideal 1.0 pace
SELECT ROUND(28.4 / 1.00, 1) AS stretch_points;
```

*Expected: commitment ≈ 21–22 points, stretch ≈ 28 points.*

## Part D — Sanity-check against real history (10 min)

In `exercise-02-notes.md`, answer in 3–4 sentences: how does your computed commitment number compare to Sprint 1's actual velocity (28 points) and Sprint 2's actual velocity (22 points)? What does it tell you that the commitment number lands close to one of these and not the other? Is that reassuring, or would you want more than two sprints of history before fully trusting this formula? Say why.

## Done when…

- [ ] Your by-hand Wes T. calculation in Part A matches Lecture 3's table (`5.69` focused days).
- [ ] Your SQL per-person breakdown in Part B matches Lecture 3's table for all five people.
- [ ] `total_focused_days` computes to approximately `28.4`.
- [ ] Both `commitment_points` (~21–22) and `stretch_points` (~28) are computed from queries, not hardcoded.
- [ ] `exercise-02-notes.md` has your written sanity-check comparison from Part D.

## Stretch

- Sprint 4's roster differs slightly (Priya N. takes a 2-day conference instead of 1 day of PTO; Dana K. has no leave this time; Wes T. comes off the on-call rotation, dropping his `meeting_hours_per_day` to `1.0` and raising his `focus_factor` to `0.75`). `INSERT` a `sprint_id = 4` version of `team_capacity_calendar` with these changes and compute Sprint 4's capacity the same way. *(Expected: total focused days ≈ 30.0, commitment ≈ 22–23 points.)* You'll need this exact number for the mini-project.
- What single change to the roster would raise Sprint 3's capacity the most — reducing Marcus's meeting load, giving Dana back her 3 leave days, or raising the team's average focus factor by 0.05? Compute all three and rank them.

## Submission

Commit `exercise-02.sql` and `exercise-02-notes.md` to your portfolio under `c39-week-04/exercise-02/`.
