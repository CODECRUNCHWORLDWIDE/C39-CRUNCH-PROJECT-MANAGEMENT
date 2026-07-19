# Week 5 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Tools to install (if you haven't already)

- **PostgreSQL 16+** — primary engine for this week's SQL. See [C33 · Crunch SQL, Week 1 resources](../../../C33-CRUNCH-SQL/curriculum/week-01-relational-model-and-select/resources.md) for install steps if you need them.
- **SQLite 3.35+** — zero-setup fallback; every query in this week's lectures runs unchanged on both engines.
- **Python 3.10+ with pandas** — `pip install pandas` — used in Lecture 3 and Exercise 3 for the re-plan calculations.

## Required reading (this week's core)

- **ProductPlan — "What Is a Now-Next-Later Roadmap?":** <https://www.productplan.com/glossary/now-next-later-roadmap/>
  *Why: the clearest short explainer of the structure Lecture 1 is built on, from the tool most associated with popularizing it.*
- **Atlassian — "How to build a killer product roadmap":** <https://www.atlassian.com/agile/product-management/product-roadmaps>
  *Why: a practitioner-voiced walk-through of outcome-based roadmapping, echoing Lecture 1, §3's feature-vs-outcome distinction.*
- **Atlassian — "Release planning in Agile":** <https://www.atlassian.com/agile/product-management/release-planning>
  *Why: a second worked treatment of release sequencing to compare against Lecture 2.*
- **Basecamp / 37signals — "Shape Up," Chapter 2, "Set boundaries":** <https://basecamp.com/shapeup/0.3-chapter-02>
  *Why: the sharpest public explanation of fixed-time/variable-scope thinking — read this right before Lecture 2, §6 and Challenge 2.*

## Reference (keep in tabs)

- **PostgreSQL docs — Window Functions:** <https://www.postgresql.org/docs/current/tutorial-window.html>
  *Why: the official reference for the `SUM() OVER (...)` running-sum pattern used throughout Lecture 2.*
- **pandas docs — `cumsum()`:** <https://pandas.pydata.org/docs/reference/api/pandas.Series.cumsum.html>
  *Why: the Python-side equivalent of the SQL running-sum pattern, used in Lecture 3's re-plan.*
- **Silicon Valley Product Group (Marty Cagan) — "Good Product Team / Bad Product Team":** <https://www.svpg.com/good-product-team-bad-product-team/>
  *Why: a well-known essay on outcome vs. output thinking, good background before Lecture 1, §3.*
- **Marty Cagan / SVPG — "The Trust Gap":** <https://www.svpg.com/the-trust-gap/>
  *Why: directly relevant to Lecture 3's framing of why mishandled change — not change itself — is what damages stakeholder trust.*

## Practice beyond the lecture case study

- **Public product roadmaps** — Linear, GitHub, and many SaaS companies publish theirs; searching "[company] public roadmap" turns up real examples for Homework Problem 1's grading exercise.
- **Real release postmortems** — searching "[company] postmortem" or "launch delay" for well-known software companies surfaces public write-ups useful for Homework Problem 5B.

## Deeper background (optional this week)

- **Scrum.org — "What is Release Planning?":** <https://www.scrum.org/resources/what-is-release-planning>
  *Why: a Scrum-specific framing of release planning, useful if your team runs formal Scrum rather than a generic Agile process.*
- **Project Management Institute — "Agile Practice Guide" overview:** <https://www.pmi.org/pmbok-guide-standards/practice-guides/agile>
  *Why: broader institutional context connecting this week's release-planning discipline back to Week 1's five-phase lifecycle model.*

## Glossary

| Term | Definition |
|------|------------|
| **Roadmap** | A strategic, outcome-focused communication artifact showing what a team intends to deliver and roughly when, at confidence appropriate to how far out it is. |
| **Now / Next / Later** | A roadmap structure that encodes confidence as bucket, not date — Now is committed, Next is sequenced, Later is directional. |
| **Outcome-based roadmap** | A roadmap written as business outcomes with initiatives as current best guesses, rather than as fixed feature commitments. |
| **Release plan** | The tactical, capacity-and-dependency-correct sequencing of backlog items into named releases with real sprint windows. |
| **Fixed-scope release** | A release where the defined scope ships as specified; the exact date has some flexibility. |
| **Fixed-date release** | A release tied to an immovable external date; scope is the lever that flexes to protect it. |
| **Velocity** | A team's historical rate of story points completed per sprint, used to forecast future capacity. |
| **Planning velocity / capacity buffer** | Historical average velocity reduced by a reserve (e.g., 15%) to account for support, on-call, and PTO not present in raw historical output. |
| **Cost of delay** | The cost of *not* shipping an item sooner, relative to its size — the basis for sequencing decisions that aren't just "smallest first." |
| **Dependency** | A relationship where one backlog item cannot ship, or cannot be verified as done, before another item completes. |
| **Milestone** | An externally meaningful checkpoint (an audit date, a partner deadline, an event) that a release plan must respect. |
| **Change control (roadmap level)** | The discipline of deciding, deliberately, whether a slip is absorbed quietly or triggers a visible re-plan. |
| **Roadmap change log** | A record of what changed on a roadmap, when, why, and who approved it — the roadmap-level equivalent of Week 1's project change log. |
| **Altitude (communication)** | The level of detail appropriate to an audience — theme/outcome for executives, item/sprint for a delivery team. |

---

*Broken link? Open an issue or PR.*
