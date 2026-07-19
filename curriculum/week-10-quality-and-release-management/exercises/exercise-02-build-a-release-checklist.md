# Exercise 2 — Build a Release Readiness Checklist

**Goal:** Build a release readiness checklist for Onboarding Copilot v1 that covers more than "the code works" — environment, data, monitoring, comms, and support — and query it the way a PM would the week of launch. By the end, you can tell, at a glance, exactly what's blocking a release and who owns clearing it.

**Estimated time:** 1 hour. **References:** [Lecture 2, §1–3](../lecture-notes/02-release-management.md).

## Setup

You need `dod_criteria` populated from Exercise 1 and the `release_checklist` table from the [week README](../README.md).

## Part A — Build the checklist

Onboarding Copilot v1's target launch is **Monday, May 4, 2026** — deliberately not a Friday (see Lecture 2, §1 if you're unsure why that matters). Write **10–12 checklist items** in `release-checklist.md`, covering **all seven categories** from Lecture 2, §1 (code freeze, environment, data, monitoring, comms, support, rollback), each with a realistic owner from the cast and a status that isn't all `done` — a checklist finished a week early isn't realistic and doesn't give you anything to practice querying.

Specific things your checklist must address, because they're specific to this release:

1. **A data item about the three provisioning systems** (email, Slack, wiki) — what does "dry-run tested" mean when three separate external-ish systems are involved?
2. **A comms item for the New Hire Council** — they're not internal engineering, so what does "briefed" look like for rotating end users who'll only interact with this tool in their first week?
3. **A support item naming who fields a new hire's "my account didn't get created" message** on day one — Onboarding Copilot has no dedicated support team the way Atlas has Jamie Okafor's org, so name a realistic real owner (hint: whoever's closest to People Ops or IT).

## Part B — Load and query it

Insert your rows into `release_checklist` with `release_name = 'onboarding-copilot-v1'`, continuing the `check_id` sequence from Lecture 2's Atlas rows (start at 15).

Write and run these three queries, saved in `checklist-queries.sql`, with output pasted into `release-checklist.md`:

4. Everything not yet `done` for Onboarding Copilot v1, sorted the same way Lecture 2 §2 sorted Atlas's (blocked first, then in_progress, then not_started, by due date).
5. A count of items per category, per `status`, for Onboarding Copilot v1 — a two-column `GROUP BY`.
6. A single query across **both** releases that shows, per `release_name`, the percentage of checklist items that are `done` — a readiness score you could report to a sponsor in one number. *(Hint: `COUNT(*) FILTER (WHERE status = 'done')` on Postgres, or a `CASE`-based `SUM` that works on both engines.)*

## Expected result (spot checks)

- Your checklist has at least one item in every one of the seven categories.
- Query 6 returns two rows, each with a readiness percentage between 0 and 100.
- If you did Part A honestly, Onboarding Copilot v1's readiness percentage should be **noticeably higher** than a mid-week Atlas snapshot would show — it's a smaller, lower-risk release with a simpler dependency graph, and your checklist should reflect that difference in maturity, not just copy Atlas's proportions.

## Done when…

- [ ] `release-checklist.md` has 10–12 items spanning all seven categories, realistic owners, and a mix of statuses.
- [ ] The three release-specific items (provisioning dry-run, New Hire Council comms, day-one support owner) are present and concrete.
- [ ] All rows are loaded into `release_checklist` with the correct `release_name`.
- [ ] `checklist-queries.sql` runs cleanly and its output is pasted into `release-checklist.md`, including the two-release comparison.

## Stretch

- Onboarding Copilot has no equivalent of Atlas's staged feature-flag rollout — new hires either have working accounts on day one or they don't. Write two sentences arguing whether this release is a big-bang case or whether there's a meaningful staged version of it anyway (hint: could it roll out to *departments*, one at a time, before an all-hands cutover?).
- Add a query that joins your `dod_criteria` `not_met` count and your `release_checklist` readiness percentage into one summary row per release — the single query a sponsor would actually want the week of launch.

## Submission

Commit `release-checklist.md` and `checklist-queries.sql` to your portfolio under `c39-week-10/exercise-02/`.
