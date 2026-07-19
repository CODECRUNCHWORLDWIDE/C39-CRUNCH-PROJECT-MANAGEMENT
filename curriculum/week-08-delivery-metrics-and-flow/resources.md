# Week 8 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Install first

- **PostgreSQL 16+** — the course's primary engine: <https://www.postgresql.org/download/> · macOS: [Postgres.app](https://postgresapp.com/). Linux: `sudo apt install postgresql` / `sudo dnf install postgresql-server`. Windows: the EDB installer.
- **SQLite 3.35+** (needs 3.30+ for `FILTER`, 3.25+ for window functions) — the zero-setup fallback: <https://www.sqlite.org/download.html>. Check with `sqlite3 --version`.
- **Python 3.10+ and pandas:**

  ```bash
  python3 -m venv .venv && source .venv/bin/activate
  pip install pandas sqlalchemy psycopg2-binary   # drop psycopg2-binary if you're SQLite-only
  ```

- **From scratch this week?** If you don't have `atlas_pm` from Weeks 6–7, this week's seed is fully self-contained — run the `CREATE TABLE`/`INSERT` blocks in the [week README](./README.md) against a fresh database (`createdb atlas_pm` / `sqlite3 atlas_pm.db`) and you have everything this week needs, no earlier tables required.

## Required reading (this week's core)

- **PostgreSQL — Window Functions Tutorial:** <https://www.postgresql.org/docs/current/tutorial-window.html>
  *Why: the plain-English introduction before the reference-manual version; read this before Lecture 2's burndown query.*
- **SQLite — Window Functions:** <https://www.sqlite.org/windowfunctions.html>
  *Why: confirms exactly which window-function features SQLite supports, and since which version.*
- **pandas — `groupby` user guide:** <https://pandas.pydata.org/docs/user_guide/groupby.html>
  *Why: Lecture 3 leans on `groupby` constantly; this is the canonical explanation of split-apply-combine.*
- **Little's Law (Wikipedia, concise and correct):** <https://en.wikipedia.org/wiki/Little%27s_law>
  *Why: the one-paragraph version of the equation this whole week orbits around.*

## Reference (keep in tabs)

- **PostgreSQL — Aggregate Expressions (`FILTER`, `HAVING`):** <https://www.postgresql.org/docs/current/sql-expressions.html#SYNTAX-AGGREGATES>
  *Why: the exact syntax and edge cases for `FILTER (WHERE ...)`, used throughout Lecture 2.*
- **PostgreSQL — `generate_series`:** <https://www.postgresql.org/docs/current/functions-srf.html>
  *Why: the "one row per calendar day" trick behind the burndown query.*
- **SQLite — Recursive Common Table Expressions:** <https://www.sqlite.org/lang_with.html>
  *Why: SQLite's substitute for `generate_series` — the `WITH RECURSIVE` calendar-day pattern.*
- **SQLite — Date and Time Functions:** <https://www.sqlite.org/lang_datefunc.html>
  *Why: `julianday()` and friends — SQLite's answer to `DATE - DATE` arithmetic.*
- **pandas — `Series.quantile`:** <https://pandas.pydata.org/docs/reference/api/pandas.Series.quantile.html>
  *Why: the exact interpolation method pandas uses for percentiles — worth knowing before you quote a p85 to a sponsor.*
- **pandas — Time deltas:** <https://pandas.pydata.org/docs/user_guide/timedeltas.html>
  *Why: `.dt.days` and friends, for every cycle-time/blocked-time calculation this week.*

## On the metrics themselves (free, practitioner-written)

- **Troy Magennis — flow-metrics and forecasting resources:** <https://www.focusedobjective.com/resources>
  *Why: the source most Monte-Carlo forecasting tooling in the industry traces back to; free spreadsheets and slide decks (read for the ideas — this course still keeps the actual computation in SQL/pandas).*
- **Daniel Vacanti — "Cumulative Flow Diagrams" (free article):** <https://actionableagile.com/cumulative-flow-diagrams/>
  *Why: the clearest free explanation of reading a CFD's bands, referenced directly in Lecture 3.*
- **Martin Fowler — "Goodhart's Law" note:** <https://martinfowler.com/bliki/> (search "Goodhart" — Fowler's bliki search)
  *Why: the general principle behind this week's "signals, not targets" rule, stated outside a PM-specific context.*
- **Atlassian — "What is cycle time?" (Jira's own definitions, useful as a real-world cross-check):** <https://www.atlassian.com/agile/project-management/metrics>
  *Why: compare Atlassian's phrasing of these same definitions against this week's — vocabulary varies slightly by vendor, and it's worth knowing the variants exist.*

## Practice beyond the seed data

- **PostgreSQL Exercises** — free, browser-based, graded `SELECT`/`GROUP BY`/window-function drills: <https://pgexercises.com/>
  *Why: more reps on the exact SQL techniques (aggregates, window functions) this week leans on.*
- **pandas "10 minutes to pandas"** — official quick tour: <https://pandas.pydata.org/docs/user_guide/10min.html>
  *Why: if `groupby`/`.dt`/`quantile` still feel unfamiliar after Lecture 3, this is the fastest re-grounding.*

## Glossary

| Term | Definition |
|------|------------|
| **Velocity** | Sum of story points completed within a given sprint. Sprint-based; needs estimation. |
| **Throughput** | Count of items completed per unit of time (usually per week). Estimation-free. |
| **Lead time** | Elapsed time from an issue's creation to its resolution — the customer's clock. |
| **Cycle time** | Elapsed time from first active work (`In Progress`) to resolution — the team's clock; includes blocked time. |
| **WIP (work in progress)** | Count of items started but not finished, at a single point in time. |
| **Little's Law** | `WIP = Throughput × Cycle Time` — the steady-state relationship linking all three. |
| **Burndown** | A chart/table of remaining work trending toward zero across a sprint. |
| **Burnup** | A chart/table with two lines — completed work and total scope — that also makes scope creep visible. |
| **Flow efficiency** | Active time ÷ total cycle time; the fraction of an issue's life actually being worked versus blocked/waiting. |
| **Cumulative flow diagram (CFD)** | A stacked chart of issue counts per status over time; band width = WIP in that status. |
| **`FILTER (WHERE ...)`** | SQL clause restricting an aggregate function to matching rows within one `GROUP BY`. |
| **Window function** | A per-row calculation (e.g. `SUM(...) OVER (...)`) that can reference other rows without collapsing the result set the way `GROUP BY` does. |
| **Goodhart's Law** | "When a measure becomes a target, it ceases to be a good measure" — the mechanism behind this week's "signals, not targets" rule. |

---

*Broken link? Open an issue or PR.*
