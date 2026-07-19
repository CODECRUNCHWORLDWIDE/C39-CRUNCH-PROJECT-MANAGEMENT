# Week 4 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Install first

- **PostgreSQL 16+** — the course's primary engine: <https://www.postgresql.org/download/> · macOS: [Postgres.app](https://postgresapp.com/). Linux: `sudo apt install postgresql` / `sudo dnf install postgresql-server`. Windows: the EDB installer.
- **SQLite 3.35+** — the zero-setup fallback; ships on macOS and most Linux already: <https://www.sqlite.org/download.html>. Check with `sqlite3 --version`.
- **Python 3.10+ with pandas** — `pip install pandas` (or `python3 -m pip install pandas` if your system separates the two). Needed for Lecture 2's distribution work and Exercise 3's range calculation.
- **A free planning-poker tool (optional, for a live team feel)** — [Scrum Poker Online](https://scrumpoker-online.org/) or [Planning Poker by Mountain Goat Software](https://www.planningpoker.com/) — no signup required for a quick session; useful if you want to run the poker mechanics with real other people instead of solo.

## Required reading (this week's core)

- **Mountain Goat Software — "What is Planning Poker?":** <https://www.mountaingoatsoftware.com/agile/planning-poker>
  *Why: the canonical, practitioner-written explanation of the mechanics Lecture 1 teaches.*
- **Scrum.org — "What are Story Points?":** <https://www.scrum.org/resources/blog/what-are-story-points>
  *Why: reinforces relative sizing vs. absolute duration from a second, independent voice.*
- **Kahneman, *Thinking, Fast and Slow*, Chapter 23 ("The Outside View"):** <https://www.penguinrandomhouse.com/books/89308/thinking-fast-and-slow-by-daniel-kahneman/>
  *Why: the single clearest explanation of inside vs. outside view forecasting available anywhere — Lecture 2 leans on it directly.*
- **Scrum.org — "What is Velocity in Scrum?":** <https://www.scrum.org/resources/what-velocity-scrum>
  *Why: the standard, careful treatment of velocity — including the warnings against using it as a productivity metric, which matters directly for Challenge 1.*

## Reference (keep in tabs)

- **Atlassian — "Sprint Planning":** <https://www.atlassian.com/agile/scrum/sprint-planning>
  *Why: a practical walkthrough of the planning ceremony itself, complementary to Lecture 3's capacity math.*
- **Mountain Goat Software — "Agile Estimating and Planning" (book landing page, with free excerpts):** <https://www.mountaingoatsoftware.com/books/agile-estimating-and-planning>
  *Why: Mike Cohn's book is the closest thing this topic has to a canonical reference; the free excerpts cover velocity and release planning in more depth than one week can.*
- **PostgreSQL — Window Functions tutorial:** <https://www.postgresql.org/docs/current/tutorial-window.html>
  *Why: the running-total pattern used in Lecture 3's sprint-pull query and the mini-project's plan-building step.*
- **pandas — `DataFrame.describe()` docs:** <https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html>
  *Why: the reference for every summary statistic Exercise 3 and Lecture 2's Python section use.*

## Deeper background (optional this week)

- **Kahneman & Tversky, "Intuitive Prediction: Biases and Corrective Procedures" (1979):** <https://apps.dtic.mil/sti/citations/ADA073679>
  *Why: the original planning-fallacy paper — dense, but the source everything else this week cites traces back to.*
- **Flyvbjerg, "From Nobel Prize to Project Management: Getting Risks Right" (2006):** <https://arxiv.org/abs/1302.3642>
  *Why: reference-class forecasting applied to real megaprojects; the same technique Lecture 2 shrinks down to a two-week sprint.*
- **Cohn, "Why Story Points are Better Than Hours":** <https://www.mountaingoatsoftware.com/blog/why-story-points-are-better-than-hours>
  *Why: a short, direct companion to Lecture 1's opening argument, from the person who popularized the technique.*

## Practice beyond this week's seed data

- **Scrum.org — Open Assessments (free, ungraded, self-check):** <https://www.scrum.org/assessments/open-assessments>
  *Why: broader Scrum-knowledge self-checks that touch estimation and sprint planning alongside everything else in the framework.*
- **Planning Poker practice decks (any of the free online tools above):** run a poker session on a backlog from a hobby project, an open-source repo's issue tracker, or even a household project list — the mechanics transfer directly outside software.

## Glossary

| Term | Definition |
|------|------------|
| **Story point** | A unit of *relative* size — how big a story is compared to a reference story — not a unit of time. |
| **Planning poker** | A structured, simultaneous-reveal estimation technique that surfaces disagreement before it becomes a hidden sprint risk. |
| **Planning fallacy** | The well-replicated tendency to underestimate duration/cost from an optimistic best-case mental scenario, which persists even with direct experience. |
| **Inside view** | Forecasting from the specifics of the task at hand — the default, more flattering, less accurate mode. |
| **Outside view** | Forecasting from how comparable efforts actually performed historically — reference-class forecasting. |
| **Reference class** | A set of comparable past efforts (your own team's history, or outside projects) used to ground a forecast in real outcomes. |
| **Focus factor** | The fraction of a person's remaining (post-leave, post-meeting) time that converts to real heads-down output. |
| **Capacity** | The real amount of focused work available in a sprint, computed from leave, meeting load, and focus factor — not the naive person-days × working-days number. |
| **Velocity** | Points a team actually delivers per sprint, measured empirically — the number that converts points into a real-world pace. |
| **Commitment** | What a team is accountable for delivering, built from capacity ÷ actual historical pace. |
| **Forecast / stretch** | The team's best-case, optimistic upside, built from capacity ÷ ideal/aspirational pace. |
| **Point inflation** | The gradual, often unintentional drift where the same real complexity gets assigned a larger point value over time, inflating velocity without real productivity change. |
| **INVEST** | The Week 3 readiness checklist (Independent, Negotiable, Valuable, Estimable, Small, Testable) — a story failing "Estimable" is this week's starting signal that it needs more conversation before pointing. |

---

*Broken link? Open an issue or PR.*
