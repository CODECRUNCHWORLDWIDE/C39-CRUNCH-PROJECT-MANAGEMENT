# Week 12 — Resources

Curated, free/official reading and the tools this week needs. Nothing here requires a paid subscription — every tool below has a genuinely usable free tier.

## Tools to install (if you haven't already)

- **PostgreSQL 16+** — <https://www.postgresql.org/download/> (primary engine for `capstone_pm`).
- **SQLite 3.35+** — <https://www.sqlite.org/download.html> (fallback; usually already installed on macOS/Linux — check with `sqlite3 --version`).
- **Python 3.10+** — <https://www.python.org/downloads/>, plus `pip install pandas numpy` for the Monte Carlo forecast in Lecture 2 and the `psycopg2-binary` driver if you're on Postgres: `pip install psycopg2-binary`.
- **git** — <https://git-scm.com/downloads> (needed for the release tag and rollback drill in Lecture 3).
- **GitHub account + GitHub Projects** — <https://github.com/join> and <https://docs.github.com/en/issues/planning-and-tracking-with-projects> (free, no paid tier needed for a single-project board).
- **Jira (free tier)**, if you prefer it to GitHub Projects — <https://www.atlassian.com/software/jira/free> (free for small teams; more than sufficient for a solo capstone).

## Project scoping and chartering (Lecture 1)

- **Re-read your own Week 1 mini-project charter** before writing this week's — `../week-01-pm-foundations-and-delivery-lifecycle/mini-project/README.md` and your own `charter.md` from that week, if you kept it.
- **"MVP" as a scoping discipline — Eric Ries on the minimum viable product (free excerpt):** <http://theleanstartup.com/principles>
- **INVEST criteria for slicing a backlog (Week 3's source, worth a second read here):** <https://www.agilealliance.org/glossary/invest/>

## Monte Carlo forecasting (Lecture 2, §5 — the week's new technique)

- **Troy Magennis — free flow-metrics and forecasting resources, spreadsheets, and talks (the origin of this technique in Agile practice):** <https://www.focusedobjective.com/resources>
- **Daniel Vacanti, *Actionable Agile Metrics for Predictability* — publisher summary and sample chapters:** <https://actionableagile.com/books/>
- **NumPy `Generator.choice` — the resampling function this week's simulation uses:** <https://numpy.org/doc/stable/reference/random/generated/numpy.random.Generator.choice.html>
- **pandas `date_range` and `reindex` — for building the zero-filled daily throughput series:** <https://pandas.pydata.org/docs/reference/api/pandas.date_range.html>

## Release gates, rollback, and closing (Lecture 3)

- **Google SRE book — Postmortem Culture (free online, the model for honest, blameless closing analysis):** <https://sre.google/sre-book/postmortem-culture/>
- **Atlassian — how to run an effective retrospective:** <https://www.atlassian.com/team-playbook/plays/retrospective>
- **`git-scm` — Tagging:** <https://git-scm.com/book/en/v2/Git-Basics-Tagging>
- **`git-scm` — Undoing things (revert, reset, checkout — the mechanics behind a rollback plan):** <https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things>
- **GitHub Docs — Releases (tagging a real, downloadable release):** <https://docs.github.com/en/repositories/releasing-projects-on-github>

## If you're using AI-assisted PM workflows (Week 11 callback)

- **GitHub Projects — automations and workflow rules:** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project>
- If you used an AI assistant to help draft your backlog, charter, or status updates this week (Week 11), keep the same discipline that week taught: AI drafts, you verify against your actual data before it goes in `charter.md`, `capstone_items`, or `delivery-report.md`. A hallucinated success criterion or a fabricated forecast number defeats the entire point of this week's data-tooling rule.

## For your own reference after the course

- **PMI — Project Management Body of Knowledge (PMBOK), free overview and glossary:** <https://www.pmi.org/pmbok-guide-standards>
- **Agile Alliance — glossary and guides (free, community-maintained):** <https://www.agilealliance.org/agile101/>
- **Scrum.org — free Scrum Guide (the canonical, short source document):** <https://scrumguides.org/>
