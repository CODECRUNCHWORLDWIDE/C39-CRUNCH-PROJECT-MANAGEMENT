# Exercise 1 — Write a Definition of Done for a Team

**Goal:** Write a real, checkable definition of done for Onboarding Copilot v1 — spanning code, tests, docs, and sign-off — and load it into `atlas_pm` so it can actually gate the release. By the end, "write a DoD" stops meaning "list some good intentions" and starts meaning "write thirteen-ish lines a specific named person can mark true or false."

**Estimated time:** 1 hour. **References:** [Lecture 1](../lecture-notes/01-definition-of-done-and-gates.md).

## Setup

Make sure `atlas_pm` has the `dod_criteria` table from the [week README](../README.md). If you haven't run the `CREATE TABLE` statements yet, do that first.

## Part A — Draft the DoD

Onboarding Copilot v1 does three things: creates a new employee's accounts (email, Slack, internal wiki), assigns their first-week checklist (including the Facilities Manager's desk assignment), and sends them a welcome email with login instructions. It touches **account provisioning** — which is exactly the surface Chen Wei has vetoed twice before.

In `dod-draft.md`, write **9–12 criteria** spanning all four categories from Lecture 1, §2. For each: the category, the exact criterion (specific enough to check true/false), whether it's required, and who owns checking it. Use the cast from [`exercises/README.md`](./README.md).

Non-negotiables your draft must include, because they follow directly from what this tool does:

1. **At least one criterion Chen Wei owns**, tied specifically to account-provisioning security — not a generic "security reviewed" line, but something concrete (e.g., "provisioned accounts follow least-privilege defaults; no new account is granted admin scope without explicit approval").
2. **At least one criterion about what happens if provisioning partially fails** — Onboarding Copilot creates accounts across three systems (email, Slack, wiki); a real DoD line has to say what "done" means if two succeed and one doesn't, not just assume all three always succeed together.
3. **At least one docs criterion** — a new-hire-facing or Facilities-facing artifact, since two different audiences depend on this tool's output.

## Part B — Load it

Insert your 9–12 rows into `dod_criteria` with `release_name = 'onboarding-copilot-v1'`, continuing the `dod_id` sequence from Lecture 1's Atlas rows (start at 14). Set realistic `status` values — don't mark everything `met`; a DoD written honestly the week before release should have at least 2–3 `not_met` or `na` rows, the same way Atlas's did.

## Part C — Query it

Save these three queries in `dod-queries.sql`, run them, and paste the actual output into `dod-draft.md`:

1. All `not_met` required criteria for Onboarding Copilot v1, ordered by category.
2. A count of criteria per category, for Onboarding Copilot v1 only.
3. A single query that returns **both** releases' `not_met` counts side by side, one row per `release_name` — this is the query a PM running two releases at once would actually want. *(Hint: `GROUP BY release_name`.)*

## Expected result (spot checks)

- Your DoD has between 9 and 12 rows, all four categories represented at least once.
- Chen Wei owns at least one row, and it's specific to provisioning, not generic.
- Query 3 returns exactly 2 rows (one per `release_name`), each with a `not_met` count.

## Done when…

- [ ] `dod-draft.md` has 9–12 criteria across all four categories, each specific enough to mark true/false without a judgment call.
- [ ] Chen Wei's provisioning-security criterion is concrete, not generic.
- [ ] At least one criterion addresses partial-failure across the three provisioning systems.
- [ ] All rows are loaded into `dod_criteria` with the correct `release_name`.
- [ ] `dod-queries.sql` runs cleanly and its output is pasted into `dod-draft.md`.

## Stretch

- Write one sentence, for each `not_met` row, on whether it's the kind of gap you'd hold the release for (like Atlas's ATLAS-131) or the kind you'd waive with a documented reason (like a genuinely low-stakes docs gap) — and justify the difference using Lecture 1, §4's ceremony-vs-signal distinction.
- Onboarding Copilot has no QA Lead of its own — Yuki Tanaka isn't on this project. Who, realistically, plays QA's role for a small internal tool with no dedicated QA headcount? Write two sentences defending your answer.

## Submission

Commit `dod-draft.md` and `dod-queries.sql` to your portfolio under `c39-week-10/exercise-01/`.
