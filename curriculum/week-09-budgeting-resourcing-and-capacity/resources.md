# Week 9 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Install / set up this week

- **PostgreSQL 16+** (primary): <https://www.postgresql.org/download/> — same install as [C33 Crunch SQL](../../../C33-CRUNCH-SQL/) and Weeks 6–7 of this course; skip if already installed.
- **SQLite 3.35+** (fallback, zero-setup): <https://www.sqlite.org/download.html> — most macOS/Linux systems already have a `sqlite3` CLI; check with `sqlite3 --version`.
- **Python 3.10+** with `pandas` and `SQLAlchemy`: <https://www.python.org/downloads/>, then `pip install pandas sqlalchemy` (add `psycopg2-binary` too if connecting `SQLAlchemy` to PostgreSQL: `pip install psycopg2-binary`). Used throughout Lecture 3, Exercise 3, and the mini-project's capacity analysis.

## Required reading (this week's core)

- **PMI — "Project Cost Management Overview":** <https://www.pmi.org/learning/library/cost-management-projects-basics-6228>
  *Why: the standard framing of budgeting, cost estimating, and cost control that Lecture 1's structure is drawn from.*
- **PMI — "Earned Value Management: A Global and Cross-Industry Perspective":** <https://www.pmi.org/learning/library/earned-value-management-systems-industry-analysis-6797>
  *Why: the industry-standard treatment of PV/EV/AC/CPI/SPI that Lecture 2 §6–7 builds on at a practical, non-certification level.*
- **PostgreSQL — Window Functions (official tutorial):** <https://www.postgresql.org/docs/current/tutorial-window.html>
  *Why: the running-cumulative-total pattern from Lecture 2 §4, explained from the database's own documentation.*
- **pandas docs — `groupby` and aggregation:** <https://pandas.pydata.org/docs/user_guide/groupby.html>
  *Why: the exact operations Lecture 3 uses to find over-allocation across a portfolio.*

## Reference (keep in tabs)

- **Atlassian — "How to create a project budget":** <https://www.atlassian.com/agile/project-management/project-budget>
  *Why: a practitioner-voiced complement to Lecture 1's cost-component breakdown.*
- **PMI — "Contingency Reserve vs. Management Reserve":** <https://www.projectmanagementdocs.com/template/project-planning/cost-management-plan/>
  *Why: a clearer treatment of the percentage-vs-risk-based contingency sizing question from Lecture 1 §4, if you want more than this course's simplified version.*
- **PostgreSQL — `WITH` Queries (Common Table Expressions):** <https://www.postgresql.org/docs/current/queries-with.html>
  *Why: the CTE pattern (`WITH plan AS (...), actual AS (...)`) used throughout Lecture 2's variance and EVM queries.*
- **pandas docs — merging and joining DataFrames:** <https://pandas.pydata.org/docs/user_guide/merging.html>
  *Why: needed for Lecture 3 and Exercise 3's plan-vs-actual comparisons across `people`, `allocations`, and `time_entries`.*
- **SQLAlchemy — Engine configuration:** <https://docs.sqlalchemy.org/en/20/core/engines.html>
  *Why: the connection pattern behind `pandas.read_sql(query, engine)`, used to pull live database tables into pandas throughout Lecture 3.*

## Practice beyond the case study

- **Real postmortems on cost overrun and burnout** — searching "[company] project over budget postmortem" or "engineering burnout retrospective" turns up public write-ups where a late-discovered burn trend or an invisible over-allocation is often named directly as a contributing cause; Homework Problem 1B uses one.
- **Public EVM calculators/worksheets** (for checking your own arithmetic only — build the actual live model in SQL + pandas per this week's data-tooling rule, not by filling in someone else's spreadsheet).

## Deeper background (optional this week)

- **PMI — *Practice Standard for Earned Value Management*** (overview page): <https://www.pmi.org/pmbok-guide-standards/foundational/practice-standard-earned-value-management>
  *Why: the fuller, certification-level treatment of EVM behind this week's simplified PV/EV/AC/CPI/SPI/TCPI/EAC introduction, if you're headed toward a PMP or similar credential later.*
- **PostgreSQL docs — Foreign Key constraints:** <https://www.postgresql.org/docs/current/ddl-constraints.html#DDL-CONSTRAINTS-FK>
  *Why: the mechanism behind Lecture 2 §8's "Failure 2" (referential integrity) — how `REFERENCES` makes a bad `budget_line_id` impossible to insert.*

## Glossary

| Term | Definition |
|------|------------|
| **BAC (Budget at Completion)** | The total approved budget for the project — the number you're forecasting against. |
| **Fully loaded rate** | What an hour of a person's time actually costs the company — salary/contract rate, benefits, and overhead already included. |
| **Contingency reserve** | A budget line reserved for known-unknown risk, sized by a percentage method, a risk-based method, or a hybrid — drawn against, not blended in. |
| **Burn** | Cumulative actual spend to date, usually broken down by category. |
| **Variance** | Plan-to-date minus actual-to-date; negative means over budget. |
| **PV (Planned Value)** | What the time-phased plan says you should have spent by a given point in the schedule. |
| **EV (Earned Value)** | What the work actually completed so far is worth, in budget dollars — `% scope complete × BAC`. |
| **AC (Actual Cost)** | What has actually been spent so far. |
| **CPI (Cost Performance Index)** | `EV / AC`. Below 1.0 means costing more than planned per unit of real progress. |
| **SPI (Schedule Performance Index)** | `EV / PV`. Below 1.0 means less value earned than the schedule called for — running behind. |
| **EAC (Estimate at Completion)** | A forecast of total project cost, commonly `BAC / CPI` (assumes current cost-efficiency continues) or `AC + (BAC − EV)` (assumes remaining work goes exactly to plan). |
| **ETC (Estimate to Complete)** | `EAC − AC` — how much more the project is forecast to cost from today forward. |
| **VAC (Variance at Completion)** | `BAC − EAC` — the forecast total overrun (or underrun) once the project finishes. |
| **TCPI (To-Complete Performance Index)** | `(BAC − EV) / (BAC − AC)` — the cost-efficiency the remaining work must hit to still land on the original BAC. |
| **Allocation** | The planned percentage of a person's capacity assigned to a given project in a given period. |
| **Over-allocation** | A person's summed allocation across all their active projects, in one period, exceeding 100% of their real capacity. |
| **Ramp-up** | The period during which a new team member (employee or contractor) is not yet at full effective productive capacity. |
| **Context-switching cost** | The real, if hard-to-pin-down, productivity tax of being actively split across two or more concurrent work contexts in the same period — not captured by percentage allocation alone. |

---

*Broken link? Open an issue or PR.*
