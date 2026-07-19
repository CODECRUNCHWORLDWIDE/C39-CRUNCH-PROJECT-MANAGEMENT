# Mini-Project — Lead a Technology Project End-to-End

> This is not a mini-project in the usual sense — it's the whole week. Everything in Lectures 1–3 and Exercises 1–3 is a piece of this; the mini-project is doing all of them, in order, against your own real, scoped project, and ending with something shipped, measured, and honestly reported on.

**Estimated time:** 18+ hours across Monday–Saturday (see the week's `README.md` schedule). This is the capstone of the entire 12-week course — treat it accordingly.

---

## The brief

Take a real technology project — chosen and scoped in Lecture 1 — from a blank board to a shipped, measured delivery. Every skill from Weeks 1–11 gets used at least once. Nothing here is simulated: the charter is a real charter for something you're really building, the risk register tracks real risks you actually hit, the forecast is run against your own real flow data, and the release is a real `git push` that a stranger could clone and run.

## Requirements

Your capstone is complete when you have produced **all** of the following, in your capstone project's own repo:

### 1. Charter and plan (Lecture 1 / Exercise 1)

- `SCOPING.md` — the full idea, the cut-down slice, and at least 2 explicit cuts.
- `charter.md` — objective, ≥3 success criteria, definition of done (first pass), scope in/out, ≥3 constraints, decision authority.
- `delivery-approach.md` — your chosen approach, defended with your project's real facts.

### 2. Backlog and board (Lecture 1, §5–6 / Exercise 1)

- 8–15 backlog items in `capstone_items`, correctly MoSCoW-prioritized, at least 4 `must` and at least 1 `could`.
- A GitHub Projects or Jira board with matching columns, a WIP limit of 1 on In Progress, and cards that track `capstone_items` throughout the week (not just at the start).

### 3. A live execution record (Lecture 2, §1–4)

- `capstone_status_history` populated daily, not reconstructed at the end — this is checkable: query it and look at whether entries cluster suspiciously on one day.
- `capstone_risks` with at least 3 real entries, at least one of which changes status (`open` → `watching`/`materialized`/`closed`) during the week, reflecting something that actually happened.
- `capstone_stakeholders` with your real (even if small) stakeholder set, and evidence of at least one real status update sent to a real person if anyone besides you is involved.

### 4. A forecast (Lecture 2, §5 / Exercise 2)

- A SQL throughput query and a 10,000-run Monte Carlo simulation against your own data, run on or before Thursday.
- `forecast.md` reporting p50/p85/p95 dates and an honest read of whether you're on track, with the lever you're pulling (if any) named explicitly.

### 5. A gated, tested release (Lecture 3, §1–3 / Exercise 3)

- `release-gate.md` with a real, run SQL query proving every `must` item is `Done`, and a real clean-checkout test result.
- `ROLLBACK.md` with a tested rollback plan — evidence (a note with the date) that you actually ran the drill, not just wrote the commands.
- A pushed `v1.0` git tag, and a working, cloneable repo that a stranger could run from your README alone.

### 6. A closing retrospective and delivery report (Lecture 3, §4–5)

- `retrospective.md` — what went well, what didn't, what you'd change, one data-backed surprise.
- `delivery-report.md` — every success criterion checked against real data, the forecast-vs-actual comparison, what got cut and why, one retrospective takeaway tied back to the data.

---

## Deliverable

Your capstone lives in its **own** repo (not this course's repo). In this course repo, submit a single file, `c39-week-12/mini-project/README.md`, containing:

1. A link to your capstone's repo and its `v1.0` release tag.
2. A copy of (or direct links to) all files listed in Requirements 1–6 above.
3. A 3–4 sentence executive summary of the whole delivery — written last, after everything else, following Challenge 2's guidance if you did that challenge.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|---------------------|
| Scoping and charter | 10% | Genuinely small, genuinely real, honest about what was cut and why |
| Backlog, board, and execution | 20% | Board and database stay in sync all week; WIP limit respected or its breaches acknowledged |
| Live risk and stakeholder tracking | 15% | Risk register actually updated mid-week in response to real events, not written once and left static |
| Forecast | 15% | Correctly built throughput series (including zero-days), correctly run simulation, honestly read |
| Release gate and rollback | 15% | Gate is a real, run SQL check, not a checklist glanced at; rollback is tested, not just written |
| Retrospective and delivery report | 20% | Honest about misses as much as wins; every claim in the report is traceable to real data |
| Overall shipped artifact | 5% | The thing actually works, cloned fresh, following your own README |

**Passing bar:** every one of the six requirement sections present and real (not simulated or backfilled), and a shipped `v1.0` that runs from a clean clone. A technically smaller or rougher project that's **entirely real and honestly reported** scores higher than a more polished one where the risk register was clearly written after the fact or the forecast used fabricated history.

---

## Stretch goals

- **Run the whole thing twice.** Ship a genuinely separate, second small capstone this week using a *different* delivery approach than your first (e.g., first one Kanban flow, second one time-boxed as a single 2-day "sprint") and write a short `comparison.md` on what changed in how you planned, executed, and forecast.
- **Bring a real second person in.** If you know someone willing to be an actual stakeholder — reviewing your charter, receiving a real status update, using the shipped tool and giving feedback — do it for real rather than role-playing it solo. Note in your delivery report what changed about the project because a real second opinion was involved.
- **Automate the daily status-history bookkeeping.** Write the small Python helper hinted at in Lecture 2, §1 that updates `capstone_items` and inserts the closing `capstone_status_history` row in one function call when a card moves — and use it for real during your own execution.

---

## Why this matters

Every course before this one ends with a project graded against a rubric someone else defined. This one ends with something that either works when a stranger clones it, or doesn't — and a report that's either honest about what happened, or isn't. That's the actual job. Twelve weeks ago you didn't know the difference between a project and an operation; this week you chartered one, ran it, measured it with your own SQL and pandas, shipped it through a real gate, and closed it out the way a project actually deserves to be closed. Keep the repo. It's the first real line in whatever you tell people you've shipped.

When done: run the [quiz](../quiz.md), and you've completed **C39 · Crunch Project Management**.
