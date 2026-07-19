# Mini-Project — Estimate a Backlog and Plan a Two-Sprint Delivery to Real Capacity

> Point the ten Sprint 3 candidates, compute real capacity for Sprint 3 *and* Sprint 4 from actual leave/meeting data, and hand Elena and Priya a two-sprint delivery plan with a stated **commitment**, a stated **stretch forecast**, and an honest range of uncertainty behind both — the exact deliverable a PM produces at the start of every real planning cycle.

**Estimated time:** 2.5–3 hours, best done Saturday after the exercises and challenges.

This is the week's capstone, and it's cumulative on purpose: Exercise 1 gave you sizes, Exercise 2 gave you a capacity formula, Exercise 3 gave you a range, and Challenge 2 gave you a two-sprint plan sketch. The mini-project asks you to assemble all four into one clean, presentable deliverable — the kind of document a real PM sends to a sponsor before a planning meeting, not scattered across four separate homework files.

---

## Deliverable

A directory in your portfolio `c39-week-04/mini-project/` containing:

1. `pointing.sql` — the final `UPDATE` statements sizing all 10 `sprint3_candidates` rows, plus the `poker_rounds` entries for at least two items with real disagreement.
2. `capacity.sql` — queries computing Sprint 3 **and** Sprint 4 capacity (person-level and team-total), pulling the historical pace factor from `atlas_story_history` rather than hardcoding it.
3. `range.py` — the pandas range calculation from Exercise 3, extended to cover **both** sprints.
4. `sprint-plan.md` — the actual plan: which items land in Sprint 3's commitment, Sprint 3's stretch, Sprint 4's commitment, Sprint 4's stretch, and which don't fit either sprint at all.
5. `report.md` — a one-page, stakeholder-facing summary (see the template below). This is the file you'd actually attach to an email to Elena and Priya.

Everything runs against the Week 4 seed tables from the [week README](../README.md). Note which engine you used.

---

## Part 1 — Point the backlog (45 min)

Finalize all 10 `sprint3_candidates` point values (reuse your Exercise 1 work, revise anything you're now less confident in after Challenge 1's lesson on inflation-proofing an estimate). Confirm nothing is `NULL`:

```sql
SELECT COUNT(*) FROM sprint3_candidates WHERE story_points IS NULL;  -- must be 0
```

## Part 2 — Compute both sprints' capacity (45 min)

Sprint 3 uses the roster already in `team_capacity_calendar`. For Sprint 4, `INSERT` a `sprint_id = 4` roster reflecting realistic sprint-to-sprint change — leave calendars are never identical two sprints running. At minimum, vary **at least two** people's `leave_days`, `meeting_hours_per_day`, or `focus_factor` from Sprint 3's numbers, and write one sentence in `report.md` justifying each change (a conference, a rotation ending, a new recurring meeting appearing).

Compute, for **each** sprint: `total_focused_days`, `commitment_points`, and `stretch_points` — following Lecture 3's formula and pulling the pace factor from `atlas_story_history`, not a hardcoded `1.32`.

## Part 3 — Build the plan (45 min)

Using priority order (`priority_rank`) and your points from Part 1, allocate the 10 candidates across four buckets: **Sprint 3 commitment**, **Sprint 3 stretch**, **Sprint 4 commitment**, **Sprint 4 stretch** — and note explicitly if any item doesn't fit even Sprint 4's stretch capacity (it stays in the backlog, unscheduled, and that's a valid, honest outcome — not a failure of the exercise).

Show the running-total SQL for each bucket (Lecture 3, section 6, is the pattern to extend).

## Part 4 — State the range (30 min)

Using `range.py`, express each sprint's realistic delivery duration as a range (optimistic ideal-days vs. history-corrected realistic days), cross-checked against the outside reference class — exactly as in Exercise 3, for both sprints this time.

## Part 5 — Write the stakeholder report (15–30 min)

`report.md`, using this structure (fill in your real numbers — don't leave the brackets):

```markdown
# Sprint 3–4 Plan — Notifications & Workspace Polish

## Capacity
- Sprint 3: commitment [N] points, stretch [N] points (total focused capacity: [N] engineer-days)
- Sprint 4: commitment [N] points, stretch [N] points (total focused capacity: [N] engineer-days)

## What's committed
[List, in priority order, the items landing in each sprint's commitment.]

## What's stretch (not promised)
[List, per sprint — clearly labeled as upside, not owed.]

## What doesn't fit this two-sprint window
[Any remaining items, with a one-line reason: priority, size, or both.]

## Confidence range
[The realistic-vs-optimistic range from Part 4, in plain language a non-technical
stakeholder can act on.]

## Biggest risk to this plan
[One sentence, grounded in this week's model — not a generic risk.]
```

---

## Milestones

- **Milestone 1 (45 min):** Part 1 — all 10 items pointed, poker rounds recorded for the two most contested.
- **Milestone 2 (45 min):** Part 2 — both sprints' capacity computed and sanity-checked against Sprint 1/2's real velocity.
- **Milestone 3 (45 min):** Part 3 — the four-bucket plan built and shown with running-total queries.
- **Milestone 4 (30–45 min):** Parts 4–5 — the range calculated and the stakeholder report written.

---

## Rules

- **No hardcoded correction factors.** `1.32`, `1.40`, and every capacity number must trace to a query against this week's tables — a number typed in from memory doesn't count, even if it happens to be right.
- **Commitment and stretch must both appear, per sprint, clearly labeled.** A plan with one blended number per sprint hasn't engaged with the week's central distinction.
- **State at least one thing that doesn't fit.** A real ten-item candidate list essentially never fits two sprints' commitment *and* stretch capacity with zero leftovers — if your plan has none, revisit your point values or your capacity math before assuming you got lucky.
- **The report is for a non-technical stakeholder.** No SQL, no raw formulas in `report.md` — plain language, real numbers, one page.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Grounded numbers | 30% | Every figure in `report.md` traces to a query in `capacity.sql`/`range.py`, no hardcoding |
| Commitment/stretch discipline | 20% | Both stated, per sprint, correctly derived (÷ actual pace vs. ÷ ideal pace) |
| Plan quality | 20% | Priority-ordered allocation, running totals shown, leftover items acknowledged |
| Stated uncertainty | 15% | The range in `report.md` is specific and explained, not a vague hedge |
| Stakeholder communication | 15% | `report.md` reads cleanly for Elena/Priya — no jargon, no raw SQL, one page |

---

## Reflection (`notes.md`, ~200 words)

1. Where did your Exercise 1 point values change (if at all) after Challenge 1's lesson on point inflation — and why?
2. Which number in this whole pipeline are you least confident in — the story points, the focus factors, or the historical pace factor — and what would you do to firm it up before a real planning meeting?
3. If Elena pushed back on the report and said "just give me one number, not a range," how would you respond, using this week's vocabulary?
4. One thing you'd want to track starting *next* sprint to make Sprint 5's capacity number even more trustworthy than this one.

---

## Why this matters

This is the actual shape of the job: a stakeholder asks a concrete question ("can we ship X by Y?"), and the honest answer requires sizing unknown work, computing real available capacity, and being upfront about what you don't know — delivered as a document someone who isn't technical can act on. Do this once for real and "sprint planning" stops being a ceremony you sit through and becomes a number you can defend under questioning, which is exactly the foundation Week 5 (risk, budget, and stakeholder communication) builds on next.

When done: push, then take the [quiz](../quiz.md) and start Week 5.
