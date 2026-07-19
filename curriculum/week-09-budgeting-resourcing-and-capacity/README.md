# Week 9 — Budgeting, Resourcing & Capacity

> **Goal:** by Sunday you can build a real project budget from labor rates and contingency, query burn and variance against that budget in PostgreSQL, forecast the cost at completion when a project is trending over, and produce a capacity/allocation plan across a portfolio that catches an over-allocated person before they burn out or silently drop a task — all of it live, queryable, and versioned in a database, never in a spreadsheet.

Welcome back to **C39 · Crunch Project Management**. You've now framed Project Atlas (Week 1), run it in Agile ceremonies (Week 2), built and sliced its backlog (Week 3), estimated and planned its sprints to capacity (Week 4), roadmapped its releases (Week 5), stood up its risk register and mapped its dependencies (Week 6), and built the stakeholder and communication machinery that keeps the right people honestly informed (Week 7). Every one of those artifacts assumes something this week finally makes explicit: **the project costs money, and the people doing the work have finite hours.** A PM who can plan a sprint but can't say "are we on budget" or "is anyone about to burn out" in one sentence each is managing half the job. This week closes that gap.

We continue the **Project Atlas** case study from Northlight Software. Atlas is a 16-week, eight-sprint initiative (two-week sprints) with a small core delivery team, a contractor ramping in, a slice of the Product Owner's time, a slice of the Platform Team Lead's time (the cross-team dependency you mapped in Week 6), and your own time as PM. It's now roughly three months in — Sprints 1 through 6 are closed, Sprint 7 is in progress — and two things are true at once: the team is *slightly* behind on delivered scope, and cost is running meaningfully over plan. Neither fact was visible from a gut feeling. Both come straight out of a query.

**Following this course's data-tooling rule:** a budget, a set of actuals, a time model, and a resourcing plan are exactly the kind of thing people reach for a spreadsheet for — and exactly the kind of thing a spreadsheet quietly ruins. A spreadsheet budget has one edit history (whoever saved last), no real foreign keys (a typo'd person name just silently fails to match), and no way to ask "show me every person allocated over 100% this month across the whole portfolio" without a fragile pile of `VLOOKUP`s and manual color-coding. Rates, actuals, and allocations are *rows* — auditable, joinable, queryable at 2am when a VP asks "why are we over budget" and you need the real number in one query, not a hunt through tabs. Every artifact this week lives in PostgreSQL (SQLite fallback), queried in SQL, with the capacity math and detection logic done in Python/pandas. That boundary is not a style preference — Lecture 2 §6 walks through, concretely, the three ways a spreadsheet budget fails that this week's schema doesn't. Spreadsheets have their place; it's [C41 Crunch Excel](../../../C41-CRUNCH-EXCEL/), not here.

## Learning objectives

By the end of this week, you will be able to:

- **Build** a project budget from the ground up — labor (rate × planned hours), tooling, vendor cost, and a defensible contingency reserve.
- **Track** actuals against plan and compute burn, variance, and burn rate as *live*, re-runnable queries, not a monthly snapshot.
- **Model** team capacity and allocation across a portfolio of projects in SQL, so "who's free" and "who's over-committed" are query results, not guesses.
- **Apply** basic earned-value ideas — planned value, earned value, actual cost — at a practical, defensible level, without needing a PMP to use them correctly.
- **Explain**, concretely, why a cost/resourcing model belongs in SQL + pandas and not Excel-as-database — and where the real boundary with C41 sits.
- **Produce** a resourcing plan that survives contact with real people's availability: leave, ramp-up time, and context-switching cost.

## Prerequisites

- Weeks 1–7 of this course, especially Week 1 (the Atlas cast and the triple constraint), Week 4 (capacity-based sprint planning — this week's per-person capacity math extends it directly), and Week 6/7 (the risk register and dependency map — this week's vendor-cost spike and Sofia Reyes's over-allocation are the same story told through money and time instead of risk score). Week 8 (delivery metrics) is helpful context but **not required** — this week's tables stand alone.
- **PostgreSQL 16+** (primary) or **SQLite 3.35+** (fallback) — the same `atlas_pm` database from Weeks 6–7. If you don't have it yet, `resources.md` has a from-scratch setup note.
- **Python 3.10+** with `pandas` installed (`pip install pandas`) — used throughout Lecture 3 and Exercise 3 for the capacity and over-allocation work.
- Comfortable with `SELECT` / `WHERE` / `GROUP BY` / basic `JOIN` SQL. This week's queries use `JOIN`, `GROUP BY`, and a couple of window functions (`SUM() OVER`) — if window functions are new, Lecture 2 §5 introduces the one pattern you need in place, no prior exposure assumed.

## Set up this week's data (do this first)

Everything this week extends `atlas_pm` with five new tables: a people/rates roster, a budget plan, actuals against it, a time model, and a portfolio allocation plan. Create the database first if you don't have it (`createdb atlas_pm` / `sqlite3 atlas_pm.db`), then run this either way:

```sql
CREATE TABLE people (
    person_id             INTEGER PRIMARY KEY,
    name                  TEXT    NOT NULL,
    role                  TEXT    NOT NULL,
    org_unit              TEXT    NOT NULL,   -- 'Engineering' | 'Product' | 'Platform Team' | 'PM'
    employment_type       TEXT    NOT NULL,   -- 'employee' | 'contractor'
    hourly_rate           NUMERIC NOT NULL,   -- fully loaded cost rate, $/hr
    weekly_capacity_hours NUMERIC NOT NULL DEFAULT 40,
    active                BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE budget_lines (
    budget_line_id INTEGER PRIMARY KEY,
    project        TEXT    NOT NULL DEFAULT 'Atlas',  -- which portfolio project this budget belongs to
    category       TEXT    NOT NULL,   -- 'labor' | 'tooling' | 'vendor' | 'contingency'
    description    TEXT    NOT NULL,
    planned_amount NUMERIC NOT NULL,
    period_start    DATE   NOT NULL,
    period_end      DATE   NOT NULL
);

CREATE TABLE actuals (
    actual_id      INTEGER PRIMARY KEY,
    budget_line_id INTEGER NOT NULL REFERENCES budget_lines(budget_line_id),
    incurred_month DATE    NOT NULL,   -- first of the month the cost lands in
    amount         NUMERIC NOT NULL,
    description    TEXT,
    source         TEXT    NOT NULL    -- 'timesheet' | 'invoice' | 'expense' | 'subscription'
);

CREATE TABLE time_entries (
    time_entry_id INTEGER PRIMARY KEY,
    person_id     INTEGER NOT NULL REFERENCES people(person_id),
    work_month    DATE    NOT NULL,    -- first of the month logged
    project       TEXT    NOT NULL,    -- 'Atlas' | 'Ledger Redesign' | 'Platform Core'
    workstream    TEXT    NOT NULL,    -- 'backend' | 'frontend' | 'platform-dependency' | 'design' | 'pm' | 'meetings'
    hours         NUMERIC NOT NULL
);

CREATE TABLE allocations (
    allocation_id INTEGER PRIMARY KEY,
    person_id     INTEGER NOT NULL REFERENCES people(person_id),
    month_start   DATE    NOT NULL,    -- first of the planned month
    project       TEXT    NOT NULL,    -- 'Atlas' | 'Ledger Redesign' | 'Platform Core'
    allocated_pct NUMERIC NOT NULL     -- 0-100, % of that person's capacity planned for this project this month
);
```

**A note on grain.** A real system logs time daily or weekly. This week uses **monthly** grain on purpose — it keeps the seed data small enough to read in one sitting while every query, formula, and pandas trick works identically at daily/weekly grain with more rows. Lecture 2 §7 says this explicitly so it isn't a silent simplification.

On SQLite, turn on foreign-key enforcement in every session before inserting: `PRAGMA foreign_keys = ON;`

You'll insert the people roster and budget plan in Lecture 1, actuals and the burn queries in Lecture 2, and the portfolio allocation data in Lecture 3.

## How to navigate this week

Work top to bottom. Each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-budgeting-a-tech-project.md](./lecture-notes/01-budgeting-a-tech-project.md) | Cost components, building a labor-dominated budget, contingency, and reading burn as a live signal | 2h |
| 2 | [lecture-notes/02-cost-and-resourcing-in-sql.md](./lecture-notes/02-cost-and-resourcing-in-sql.md) | The full schema in PostgreSQL; querying burn, variance, and forecast; the spreadsheet-vs-database boundary in detail | 2h |
| 3 | [lecture-notes/03-capacity-and-allocation.md](./lecture-notes/03-capacity-and-allocation.md) | Portfolio capacity and allocation in pandas; finding over-allocation; a resourcing plan that respects leave and ramp-up | 2h |
| 4 | [exercises/exercise-01-build-a-budget-schema.md](./exercises/exercise-01-build-a-budget-schema.md) | Design and populate a budget + actuals schema for a second project | 1h |
| 5 | [exercises/exercise-02-query-burn-and-variance.md](./exercises/exercise-02-query-burn-and-variance.md) | Query burn, variance, and burn rate against Atlas's real budget | 1h |
| 6 | [exercises/exercise-03-detect-over-allocation.md](./exercises/exercise-03-detect-over-allocation.md) | Find every over-allocated person-month across the portfolio with pandas | 1h |
| 7 | [challenges/challenge-01-forecast-at-completion.md](./challenges/challenge-01-forecast-at-completion.md) | Forecast Atlas's cost at completion two different ways and reconcile them | 1.5h |
| 8 | [challenges/challenge-02-reallocate-under-a-cut.md](./challenges/challenge-02-reallocate-under-a-cut.md) | Re-resource the whole portfolio after a 20% headcount cut | 1.5h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Model a project's cost, burn, and resourcing end-to-end in PostgreSQL + pandas | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week | 4h |
| 11 | [quiz.md](./quiz.md) | 14 self-check questions + answer key | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install | — |

## Weekly schedule

The schedule below adds up to approximately **21 hours** for the full-time pace.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Budgeting fundamentals; build the budget | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | Schema in SQL; querying burn and variance | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Capacity and allocation in pandas | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Challenge: forecast-at-completion | 0h | 0h | 1.5h | 0.5h | 1h | 1h | 4h |
| Friday | Challenge: reallocate under a cut | 0h | 0h | 1.5h | 0.5h | 0h | 1.5h | 3.5h |
| Saturday | Mini-project — budget, burn, capacity plan | 0h | 0h | 0h | 0h | 0h | 2.5h | 2.5h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **3h** | **3.5h** | **4h** | **5h** | **~21h** |

## By the end of this week you can…

- Build a labor-dominated project budget from real rates and planned hours, with a contingency reserve you can defend the size of.
- Write a query that returns "are we over budget, and by how much" for any category, any month, any point in the project — no annual guess, no stale snapshot.
- Forecast the cost at completion of a slipping project two independent ways, and explain why they should roughly agree.
- Query a portfolio of people and projects to find exactly who is over-allocated, this month, and why — including the quiet double-booking that causes a cross-team dependency to slip.
- Say, specifically, why this all lives in a database instead of a spreadsheet — not as a rule you were told, but as three concrete failure modes you can name.
- Re-resource a portfolio under a real constraint (a headcount cut, a leave calendar, a ramp-up curve) without guessing.

## Up next

Week 10 — Quality & Release Management — once you can say what the project costs and who's actually available to build it, the next question is whether what ships is actually ready: a definition of done, a release gate, and a rollback plan for when it isn't.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
