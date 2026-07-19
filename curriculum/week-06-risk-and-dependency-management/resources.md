# Week 6 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Install / set up this week

- **PostgreSQL 16+** (primary): <https://www.postgresql.org/download/> — same install as [C33 Crunch SQL](../../../C33-CRUNCH-SQL/); skip if already installed.
- **SQLite 3.35+** (fallback, zero-setup): <https://www.sqlite.org/download.html> — most macOS/Linux systems already have a `sqlite3` CLI; check with `sqlite3 --version`.
- **Python 3.10+** with `pandas`: <https://www.python.org/downloads/>, then `pip install pandas`. Used for Lecture 3's critical-path script.

## Required reading (this week's core)

- **PMI — "Risk Management Overview for Project Managers":** <https://www.pmi.org/learning/library/risk-management-overview-project-managers-7113>
  *Why: the industry-standard framing of risk identification and response strategy that Lecture 1's four response types are drawn from.*
- **Atlassian — "RAID log explained":** <https://www.atlassian.com/work-management/project-management/raid-log>
  *Why: a practitioner-voiced walk through Risks/Assumptions/Issues/Dependencies as one connected model, matching Lecture 1 §5.*
- **PMI — "Managing Project Dependencies":** <https://www.pmi.org/learning/library/managing-inter-project-dependencies-6046>
  *Why: a second worked treatment of cross-team/cross-project dependency coordination, useful alongside Lecture 2.*
- **PMI — "Critical Path Method: A Project Scheduling Technique":** <https://www.pmi.org/learning/library/critical-path-method-scheduling-technique-8025>
  *Why: the canonical explanation of forward pass / backward pass / float that Lecture 3's math is built on.*

## Reference (keep in tabs)

- **Wikipedia — "Critical path method"** (clear worked examples with network diagrams): <https://en.wikipedia.org/wiki/Critical_path_method>
  *Why: useful for seeing the forward/backward pass drawn as a network diagram, a visual complement to this week's tables.*
- **Atlassian — "Cross-team collaboration at scale":** <https://www.atlassian.com/agile/agile-at-scale/cross-team-collaboration>
  *Why: practical patterns for the coordination-cost problem in Lecture 2 §3, from teams running many interdependent squads.*
- **PMI — "Critical Chain Project Management":** <https://www.pmi.org/learning/library/critical-chain-project-management-buffer-8003>
  *Why: a related but distinct buffering philosophy (Goldratt's Critical Chain) worth knowing about alongside Lecture 3 §7's simpler buffering guidance.*
- **PostgreSQL docs — `CHECK` constraints:** <https://www.postgresql.org/docs/current/ddl-constraints.html>
  *Why: how to enforce valid probability/impact ranges (1–5) directly in the schema, referenced in Lecture 1 §7.*
- **pandas docs — `DataFrame` basics:** <https://pandas.pydata.org/docs/user_guide/dsintro.html>
  *Why: if the `pandas` usage in Lecture 3 §6's script is unfamiliar, this is the fastest on-ramp.*

## Practice beyond the case studies

- **Real postmortems** — searching "\[company] postmortem" or "incident retrospective" turns up dozens of public write-ups where an unmanaged risk or an invisible dependency chain is often named directly as a root cause; Homework Problem 1B uses one.
- **Public project-risk-register templates** (for inspiration on categories and structure only — build the actual thing in SQL per this week's data-tooling rule, not by filling in someone else's spreadsheet template).

## Deeper background (optional this week)

- **PMI — *The Standard for Risk Management in Portfolios, Programs, and Projects*** (overview page): <https://www.pmi.org/pmbok-guide-standards/foundational/risk-management>
  *Why: the fuller, more formal treatment behind this week's simplified probability × impact scoring, if you're considering a PMP or similar certification later.*
- **Eliyahu Goldratt — *Critical Chain*** (book, not free, mentioned for context): the origin of the "critical chain" buffering philosophy referenced above — not required, but a well-known extension of this week's critical-path material for anyone going deeper into scheduling theory.

## Glossary

| Term | Definition |
|------|------------|
| **Risk** | Something that might happen and, if it does, would hurt scope, schedule, cost, or quality. |
| **RAID** | Risks, Assumptions, Issues, Dependencies — four related categories tracked in one living log. |
| **Assumption** | Something treated as true without full confirmation; becomes a risk or issue if it turns out false. |
| **Issue** | A risk (or failed assumption) that has already happened and needs an active response now. |
| **Probability** | 1–5 scale estimating how likely a risk is to occur. |
| **Impact** | 1–5 scale estimating how severe the consequence would be if a risk occurs. |
| **Risk score** | Probability × impact; used to rank and triage, not as precise math. |
| **Avoid** | A risk response that changes the plan so the risk can't happen at all. |
| **Mitigate** | A risk response that reduces probability and/or impact without eliminating the risk. |
| **Transfer** | A risk response that shifts who bears the consequence (e.g., via SLA or contract). |
| **Accept** | A conscious, documented decision to take no extra action on a risk. |
| **Internal dependency** | A dependency between tasks or people inside one team. |
| **Cross-team dependency** | A dependency on a team, vendor, or party outside your direct control. |
| **Coordination cost** | The discovery, alignment, interface-risk, and schedule-coupling overhead a cross-team dependency adds beyond its raw task duration. |
| **Coupling** | How tightly one team's or task's progress depends on another's; reducing it shrinks a dependency's blast radius. |
| **Critical path** | The longest chain of dependent tasks from start to finish in a schedule; every task on it has zero slack. |
| **Forward pass** | The calculation of each task's earliest start (ES) and earliest finish (EF). |
| **Backward pass** | The calculation of each task's latest finish (LF) and latest start (LS), working backward from the project end date. |
| **Slack / float** | LS − ES (or LF − EF); the amount a task can slip without delaying the project's finish date. |
| **Buffer** | Deliberate extra schedule time placed to protect against a specific, identified risk — most useful on critical-path tasks, wasted on tasks with existing slack. |

---

*Broken link? Open an issue or PR.*
