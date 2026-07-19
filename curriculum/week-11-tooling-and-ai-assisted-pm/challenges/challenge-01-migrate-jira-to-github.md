# Challenge 1 — Migrate a Project from Jira to GitHub Projects

**Time:** ~2 hours. **Difficulty:** Hard. **No single right answer.**

## The scenario

Northlight's engineering leadership has made the call: every team standardizes on GitHub Projects by end of quarter. Sofia Reyes's Platform team is the last holdout, still running `PLAT` on Jira. Marcus has been asked to write the migration plan — not because he owns Sofia's team, but because Atlas's own SLA dashboard work is *blocked* on PLAT's backlog (PLAT-505), so he has the clearest incentive in the company to get this right and get it done. This challenge is that plan, made concrete: a real schema mapping, a real migration script, and an honest accounting of what doesn't survive the move.

## Your task

Work against this week's `jira_issues` seed (standing in for a full export of PLAT) and `board_items`' schema (standing in for the target). Produce three things in a `challenge-01/` folder:

### 1. A field-mapping table (`mapping.md`)

For every `jira_issues` column, state exactly where it goes in `board_items` — a direct column, a custom field, a label, or "does not migrate" with a reason. Use Lecture 2's concept-map table as your starting point, but go further: it left the Epic question open, and it didn't address `reporter`, `story_points`, or `Rank` at all. You have to.

Specifically resolve these, each in 2–3 sentences:

- **`story_points` (numeric) → GitHub Projects' `Size` (single-select: XS/S/M/L/XL) or a new numeric field?** Argue for one. If you pick single-select, you need a numeric-to-bucket rule — write it down as an explicit rule (e.g., "1–2 pts → S", not "roughly maps to S").
- **`reporter` — does GitHub Projects have anything like it?** (Hint: GitHub issues have an `author`, set automatically at creation, not a separately-editable field like Jira's `reporter`. What does that difference mean for issues you're migrating that were *reported* by one person but *filed* into GitHub by someone else — you, doing the migration?)
- **Jira's workflow-enforced transitions (Lecture 2, Section 3 — e.g., `To Do` can't jump straight to `Done`) — what replaces that enforcement in GitHub Projects, which has no such concept natively?** Pick one: accept the loss and rely on team discipline, or design a lightweight GitHub Action that checks status transitions on `updateProjectV2ItemFieldValue` events and rejects illegal ones. Either is acceptable — defend your choice.
- **Jira's `Rank` (manual drag order, no source column) — how do you seed an initial order for 14 migrated items with no natural rank?** State the rule you'd use (e.g., "priority DESC, then created ASC" as a one-time seed order) and say why a human should expect to re-rank shortly after migration regardless.

### 2. A migration script (`migrate.sql` + `migrate.py`)

Write a SQL/Python migration that reads every row of `jira_issues` and produces the equivalent `board_items` rows, following your own mapping table exactly — no field mapped by hand that isn't automated in the script. At minimum:

- `story_points` → `size` (or a new numeric field, per your decision above).
- `sprint` (`'PLAT Sprint 15'`) → `iteration` (a name your `board_items` schema can hold, e.g., `'PLAT Sprint 15'` unchanged, or renumbered to align with Atlas's `Sprint 8` naming — your call, stated).
- `assignee` → `assignee` directly.
- `labels` → `labels`, with an added `migrated-from-jira` label so the origin is traceable after the fact.
- Every migrated `status` value maps onto one of `board_items`' five statuses exactly — if Jira's status set doesn't line up 1:1 (it doesn't; check), state your remapping explicitly in `mapping.md`, don't silently coerce it in the script.

### 3. A reconciliation report (`reconciliation.sql` output, pasted into `mapping.md`)

After running the migration, write and run a SQL query that proves the migration didn't silently drop or duplicate anything:

```sql
-- Row count parity
SELECT
    (SELECT COUNT(*) FROM jira_issues) AS jira_count,
    (SELECT COUNT(*) FROM migrated_board_items WHERE 'migrated-from-jira' = ANY(string_to_array(labels, ','))) AS migrated_count;
```

*(Adapt the label-check to your engine — SQLite has no `string_to_array`; a `LIKE '%migrated-from-jira%'` is a fine substitute for this reconciliation check.)*

The two counts must match. If they don't, find the dropped or duplicated row before you call the migration done — this is the same discipline as a `COUNT(*)` sanity check after any `INSERT` this course has taught since Week 1.

## Constraints

- Script it. A migration plan that only exists as prose is not a passing answer to this challenge.
- Every "does not migrate cleanly" call in your mapping table needs a stated reason, not just a flag.
- Assume Sofia's team keeps working in Jira **during** the migration window — your plan should say one sentence about how you'd handle issues created or changed in `PLAT` after your export snapshot but before cutover (you don't have to build this, just name the problem and a plausible approach: a second delta sync, a hard cutover with a freeze window, etc.).

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|---------------|
| Field mapping | Vague ("story points map to size") | Explicit rule, every edge case named |
| Loss honesty | Claims a clean 1:1 migration | Names exactly what's lost (workflow enforcement, `Rank`, `reporter` semantics) and what you'd do about it |
| Script correctness | Hand-waves the transform | Runs, and the reconciliation count matches |
| Cutover thinking | Ignores the "team keeps working" problem | Names it and picks a real, defensible approach |

## Submission

Commit `challenge-01/` (containing `mapping.md`, `migrate.sql`/`migrate.py`, and the reconciliation query + output) to your portfolio under `c39-week-11/challenge-01/`.
