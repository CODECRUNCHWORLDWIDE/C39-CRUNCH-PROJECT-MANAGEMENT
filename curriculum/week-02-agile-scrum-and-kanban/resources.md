# Week 2 — Resources

Curated, free, mostly official. Read these to go deeper than the lectures, not instead of them.

## Install first (if you haven't already)

This week's data setup needs a working SQL engine and, optionally, Python + pandas for the flow-diagram sections of Lecture 3 and the mini-project's stretch goal.

### PostgreSQL 16+ (primary engine for this course)

- **macOS:** `brew install postgresql@16 && brew services start postgresql@16`
- **Windows:** installer at <https://www.postgresql.org/download/windows/>
- **Linux (Debian/Ubuntu):** `sudo apt install postgresql`
- Verify: `psql --version` should report 16 or higher.

### SQLite 3.35+ (zero-setup fallback)

- **macOS/Linux:** usually preinstalled; verify with `sqlite3 --version`.
- **Windows:** download the precompiled binary at <https://sqlite.org/download.html>.
- If your version is older than 3.35, some `ORDER BY`/window-function syntax used elsewhere in this course won't work — upgrade rather than work around it.

### Python + pandas (for Lecture 3 §5e and the mini-project's stretch goal)

```bash
python3 -m pip install pandas matplotlib
```

Not required to finish this week's core deliverables — only the cumulative-flow-diagram material touches it.

## The Agile Manifesto — primary sources

- **The Agile Manifesto (original site, four values):** <https://agilemanifesto.org/>
- **The Twelve Principles behind the Agile Manifesto:** <https://agilemanifesto.org/principles.html>
- **Martin Fowler — "The New Methodology" (the history and reasoning behind the Manifesto):** <https://martinfowler.com/articles/newMethodology.html>
- **Martin Fowler — "The State of Agile Software in 2018" (a candid look at where "Agile" drifted from its origins):** <https://martinfowler.com/articles/agile-aus-2018.html>

## Scrum — official and near-official

- **Scrum.org — The Scrum Guide (official, free, ~14 pages):** <https://scrumguides.org/scrum-guide.html>
- **Scrum.org — "What is a Sprint Retrospective?":** <https://www.scrum.org/resources/what-is-a-sprint-retrospective>
- **Atlassian — "What is a Scrum Master?":** <https://www.atlassian.com/agile/scrum/scrum-master>
- **Atlassian — "Daily Standup Meetings":** <https://www.atlassian.com/agile/scrum/standups>
- **Scrum.org — "What is the Definition of Done?":** <https://www.scrum.org/resources/what-definition-done>

## Kanban — official and near-official

- **Kanban University — "What is Kanban?" (the official guide):** <https://kanban.university/kanban-guide/>
- **Atlassian — "Kanban vs. Scrum":** <https://www.atlassian.com/agile/kanban/kanban-vs-scrum>
- **Atlassian — "What is a Kanban board?":** <https://www.atlassian.com/agile/kanban/boards>
- **David J. Anderson (creator of the Kanban Method) — "Kanban Explained in Under 500 Words":** <https://kanbanzone.com/resources/kanban/kanban-explained/>

## Flow metrics and the theory behind WIP limits

- **Wikipedia — "Little's Law" (the queueing-theory relationship behind §4):** <https://en.wikipedia.org/wiki/Little%27s_law>
- **Actionable Agile / Prokanban — "Cycle Time vs. Lead Time":** <https://prokanban.org/cycle-time-lead-time/>
- **Nave — "Cumulative Flow Diagrams Explained":** <https://www.getnave.com/blog/cumulative-flow-diagram/>

## Tools worth knowing (not required this week)

- **Real Scrum/Kanban boards:** GitHub Projects (free, and the tool this course's own PM assignments use — see Week 1's mini-project), Trello (free tier, simple Kanban), Jira (industry-standard, free for small teams).
- This week's own board/backlog work is done in **SQL against `atlas_backlog`**, not in one of these tools — the point is to understand the data model underneath any board tool, so that whichever tool you use professionally, you know what it's actually tracking.

## If you only read three things this week

1. The Scrum Guide (official, ~20 minutes) — it's short, free, and the primary source everything in Lecture 2 is grounded in.
2. Atlassian's "Kanban vs. Scrum" — the clearest short comparison available, and a good sanity check against Lecture 3 §6.
3. The "Cycle Time vs. Lead Time" piece — the single most commonly confused pair of terms in this whole week, worth nailing down cold.
