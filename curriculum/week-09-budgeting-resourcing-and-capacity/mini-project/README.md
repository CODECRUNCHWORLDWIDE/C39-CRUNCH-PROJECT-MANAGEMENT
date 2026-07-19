# Mini-Project — Model a Project's Cost, Burn & Resourcing End-to-End

> Build a complete budget, burn analysis, forecast, and resourcing plan for one real (or realistic) project — entirely in PostgreSQL and pandas, with a short written case for why none of it lives in a spreadsheet. This is the week's capstone: everything from Lectures 1–3 assembled into one deliverable you could actually hand to a sponsor.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

This is the artifact this week has been building toward. A sponsor doesn't want to hear "the budget's fine" or "we're a little over" — they want a number, a trend, and a plan, all defensible on request. You've built every piece of that this week: a budget from real rates, a burn/variance query set, an earned-value forecast, and a portfolio-aware resourcing plan. The mini-project asks you to do it once, cleanly, end to end, for a project that's genuinely yours to model — either **Atlas itself, extended one more month** or **a real project from your own work, school, or a side project**, your choice.

---

## Choose your project

**Option A — Extend Atlas.** Continue the case study: build December's actual close (you have partial data from Challenge 1 Part B — decide how the rest of December landed, plausibly, and finalize it), and produce the final BAC-vs-actual picture for the full project.

**Option B — Model something real.** Pick a real project you have visibility into (work, school group project, open-source contribution, a personal project with a real time/cost tradeoff). Build a *smaller* but *real* schema: your own roster (even if it's just you and a couple of collaborators), your own realistic budget (even if some of it is estimated hours × an assumed rate rather than real invoices), and your own resourcing picture. Smaller in scope than Atlas is fine — the discipline is what's being graded, not the dollar amount.

State which option you chose at the top of your `report.md`.

---

## Deliverable

A directory in your portfolio `c39-week-09/mini-project/` containing:

1. **`schema.sql`** — your full `people`, `budget_lines`, `actuals`, `time_entries`, and `allocations` tables (reuse this week's schema as-is, or adapt column names if Option B's context genuinely requires it — state any changes and why).
2. **`seed.sql`** — every `INSERT` populating those tables for your chosen project.
3. **`queries.sql`** — every query you ran to produce the numbers in your report: burn by category, variance vs. plan-to-date, a running cumulative total, PV/EV/AC/CPI/SPI, and at least one EAC forecast.
4. **`capacity_analysis.py`** — your pandas script: normalized planned hours, at least one over-allocation check, and a plan-vs-actual comparison for at least one person/month.
5. **`report.md`** — the human-readable deliverable (structure below).
6. **`notes.md`** — a short reflection (see the end).

---

## What `report.md` must contain

Write it as if Priya Chen (or your real project's equivalent stakeholder) is going to read it cold, with no context beyond what's on the page.

### 1. The budget (from Lecture 1's discipline)

- Total BAC, broken down by category (labor / tooling / vendor / contingency), with a one-paragraph justification for your contingency sizing (percentage method, risk-based method, or a stated hybrid).

### 2. Burn and variance

- A burn-by-category table, as of your chosen "as of" date.
- A variance table (plan-to-date vs. actual-to-date), with a one-sentence read of which category is the biggest concern and why — dollar exposure, percentage overrun, or both.
- One running-cumulative-total chart or table (can be markdown, doesn't need to be a rendered chart) that shows whether any category's overrun is trending better, worse, or holding steady.

### 3. Earned value and forecast

- PV, EV, AC, CPI, SPI as of your as-of date, with your source for "% scope delivered" stated explicitly (story points, task count, milestones — whatever's honest for your project).
- At least one EAC calculation, with the method named, and a one-paragraph plain-English translation of what it means for whoever's paying.

### 4. Resourcing and capacity

- Your roster with rates/capacity.
- At least one **over-allocation finding** — if your real project genuinely has none (a small enough team that nobody's double-booked), say so explicitly and show the check that proves it, rather than skipping the section.
- One plan-vs-actual comparison (planned hours vs. logged hours) for at least one person, with a one-sentence read of what the gap (if any) tells you.

### 5. Why this isn't a spreadsheet

- In your own words (not copied from Lecture 2 §8), 3–4 sentences naming **one specific way** your own project's data would have broken, or already has broken, in a spreadsheet — using a real example from your own schema/data if you can (a formula that would silently miscount, a cross-cutting question that would need a fragile array formula, an edit-history problem). Generic restatement of the lecture without a project-specific example is a weak answer here.

---

## Rules

- **Every number in `report.md` must trace to a query in `queries.sql` or a computation in `capacity_analysis.py`.** No hand-typed numbers that don't come from the artifacts you're submitting.
- **State every judgment call.** "% scope delivered," contingency sizing, and any assumption about missing real-world data (Option B especially) each need one stated sentence of reasoning.
- **The database is the source of truth.** If `report.md` and `queries.sql`/the live tables disagree, that's treated as a bug in your submission, not a rounding difference to wave away.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Budget construction | 20% | Categories, contingency sizing, and total all defensible and internally consistent |
| Burn/variance/trend | 20% | Correct arithmetic; the running-total check genuinely used to say something about trend, not just computed and ignored |
| Earned value & forecast | 20% | PV/EV/AC/CPI/SPI computed correctly; EAC method named and its assumption stated |
| Resourcing & capacity | 20% | A real over-allocation check with a real (or honestly-absent) finding; plan-vs-actual comparison included |
| Spreadsheet argument | 10% | A specific, project-grounded example — not a restatement of the lecture |
| Traceability | 10% | Every reported number reproducible from the submitted SQL/Python |

---

## Reflection (`notes.md`, ~200 words)

1. Which number in your report surprised you most once you actually computed it, versus what you expected before running the query?
2. If you chose Option B, what was hardest to model honestly with real (or estimated) data — rates, hours, or scope percentage?
3. Which of this week's three skills (budgeting, burn/EVM, capacity/allocation) do you trust yourself to do cold, under pressure, in a real status meeting? Which still needs a lecture open beside you?
4. One thing you'd add to this week's schema if you were going to run it on a real project for real, past this course.

---

## Why this matters

This mini-project is the shape of the job once you're actually running delivery, not just planning it: someone asks "are we on budget and are we staffed right," and you produce a trustworthy, traceable answer from live data in minutes, not a scramble through last quarter's spreadsheet. Keep this database — Week 10's release management work and the Week 12 capstone both assume you can still answer "what did this cost, and who worked on it" on demand.

When done: push, then take the [quiz](../quiz.md) and start Week 10 — Quality & Release Management.
