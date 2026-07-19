# Challenge 1 — Build a Flow Dashboard from Raw Exports

**Time:** ~90 minutes. **Difficulty:** Medium. **Deliverable is a reusable set of SQL views, not a one-off answer.**

## The scenario

Marcus Diallo doesn't want to hand-write these queries every Monday. He wants to run one setup script once, then have Priya Chen (or anyone else) get a current answer with a single `SELECT` against a named view — no copy-pasting a five-CTE query out of a lecture note every time someone asks "what's our velocity." Your job is to package Week 8's queries into a small, reusable **flow dashboard**: a handful of SQL views that any future query, report, or dashboard tool can just `SELECT * FROM`.

This is also a realistic day-one task on the job: raw exports and ad-hoc queries are the *research* phase; a dashboard is the *product* someone else depends on. The difference is discipline — clear names, no query that silently breaks when a new sprint starts, and no view that needs to be manually edited every week.

## Your task

Write a single `dashboard.sql` file that, run once against `atlas_pm`, creates the following views. For each, state in a one-line SQL comment above the `CREATE VIEW` exactly what question it answers.

1. **`v_velocity_by_sprint`** — sprint, committed points, completed points, and a boolean/flag for whether the sprint is closed (`end_date < CURRENT_DATE`) or still open. *(This flag matters — Sprint 7's `8` should never be silently compared to Sprint 3's `21` as if they were the same kind of number, and the view itself should make that distinction visible, not just a comment in a lecture.)*
2. **`v_throughput_by_week`** — calendar week, issues resolved that week, and a trailing 4-week rolling average.
3. **`v_current_wip`** — one row per currently-open issue (`In Progress`, `In Review`, `Blocked`), with `age_in_current_status_days` and `total_age_days` computed live off `CURRENT_DATE` — not hardcoded to the 2026-04-06 snapshot, so the view stays correct every day it's queried.
4. **`v_cycle_time`** — one row per `Done` issue: `issue_key`, `issue_type`, `story_points`, `cycle_time_days`, `days_blocked`, `flow_efficiency`.
5. **`v_blocked_leaderboard`** — every issue that has *ever* spent time `Blocked` (including one still open right now — fix the `NULL`-`left_at` gap Lecture 2 flagged), sorted by total blocked days descending.

## Constraints

- **Every view must still be correct next sprint**, not just for this snapshot. If your `v_current_wip` view has `2026-04-06` typed anywhere in it, that's a bug, not a feature — use `CURRENT_DATE`.
- No view may reference another view's *output as a hardcoded literal* — build on top of other views with `SELECT ... FROM v_something` if that's cleaner, don't copy-paste logic between them.
- Keep every view queryable with a bare `SELECT * FROM v_whatever;` — no required parameters, no `WHERE` the caller is forced to supply just to get a sane result.

## After you build it

Run these three checks against your own dashboard and record the answers in `dashboard-notes.md`:

1. `SELECT * FROM v_velocity_by_sprint ORDER BY sprint;` — which sprints are flagged closed vs. open, and does the flag match what you'd expect given today's date in the data (2026-04-06, mid-Sprint-7)?
2. `SELECT * FROM v_current_wip ORDER BY age_in_current_status_days DESC;` — which issue is oldest in its current status?
3. `SELECT * FROM v_blocked_leaderboard;` — does `ATLAS-126` (the one still open and blocked) now correctly appear, unlike in Lecture 2's version?

## Hints

<details>
<summary>On making <code>v_current_wip</code> date-independent</summary>

Don't filter `issue_status_history` by a literal date at all for this one — you don't need a "point in time" query here, you need "right now," which is exactly what `issues.current_status` and the open (`left_at IS NULL`) row in `issue_status_history` already give you. Join `issues` to the open history row, and compute ages against `CURRENT_DATE`.

</details>

<details>
<summary>On fixing the blocked-leaderboard NULL gap</summary>

Lecture 2 named the bug: `left_at - entered_at` is `NULL` when `left_at IS NULL`. Wrap it: `COALESCE(left_at, CURRENT_DATE) - entered_at` (Postgres) or the `julianday(...)` equivalent on SQLite. Now a currently-blocked issue's *elapsed-so-far* blocked time counts, and it'll keep growing correctly every day the view is queried again.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Correctness | Views work once, against today's exact data | Views stay correct as `CURRENT_DATE` moves forward and new sprints/issues are added |
| Reusability | Copy-pasted logic across views | Views build on each other; no duplicated CTEs |
| The open-sprint caveat | Velocity view treats all sprints identically | Sprint 7 (or whichever is open) is visibly flagged, not silently compared |
| The `NULL`-gap fix | Blocked leaderboard still drops in-flight blocks | `ATLAS-126` (or whichever issue is currently blocked) correctly appears with its partial duration |
| Documentation | No comments, unclear what each view answers | Each view has a one-line comment stating its question |

## Submission

Commit `dashboard.sql` and `dashboard-notes.md` to your portfolio under `c39-week-08/challenge-01/`.
