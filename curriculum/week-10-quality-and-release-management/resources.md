# Week 10 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Required reading (this week's core)

- **Scrum.org — "Definition of Done":** <https://www.scrum.org/resources/what-definition-done>
  *Why: the canonical framing of a DoD as a fixed, team-owned checklist — the foundation Lecture 1 builds Atlas's real GA DoD from.*
- **Google SRE Book — "Release Engineering":** <https://sre.google/sre-book/release-engineering/>
  *Why: the industry-standard treatment of release readiness, cutover discipline, and the organizational muscle behind Lecture 2.*
- **Martin Fowler — "CanaryRelease":** <https://martinfowler.com/bliki/CanaryRelease.html>
  *Why: the original, precise framing of staged rollout patterns Lecture 2, §4 compares against big-bang releases.*
- **Google SRE Book — "Postmortem Culture: Learning from Failure":** <https://sre.google/sre-book/postmortem-culture/>
  *Why: the definitive treatment of blameless review — read this before Challenge 2, since staying genuinely blameless under pressure is harder than it sounds until you've seen it modeled well.*

## Reference (keep in tabs)

- **Atlassian — "Definition of Done":** <https://www.atlassian.com/agile/project-management/definition-of-done>
  *Why: a shorter, practitioner-voiced companion to the Scrum.org piece, with more worked examples.*
- **Martin Fowler — "BlueGreenDeployment":** <https://martinfowler.com/bliki/BlueGreenDeployment.html>
  *Why: the fuller treatment of blue-green cutover, one of the four staged patterns from Lecture 2, §4.*
- **Atlassian — "Feature flags":** <https://www.atlassian.com/microservices/microservices-architecture/feature-flags>
  *Why: why decoupling "shipped" from "live" is the single biggest lever for fast, safe rollback (Lecture 3, §1).*
- **Martin Fowler — "Evolutionary Database Design":** <https://martinfowler.com/articles/evodb.html>
  *Why: the full treatment of the "expand, contract" migration pattern referenced in Lecture 3, §1.*
- **Etsy — "Blameless PostMortems and a Just Culture":** <https://www.etsy.com/codeascraft/blameless-postmortems/>
  *Why: one of the earliest and most widely cited public arguments for blameless review — good grounding before Challenge 2.*
- **Atlassian — "Incident postmortem":** <https://www.atlassian.com/incident-management/postmortem>
  *Why: a practical template structure, useful to compare against your own Challenge 2 / mini-project review template.*

## Practice beyond the lecture case study

- **Real public postmortems** — searching "postmortem" or "incident report" alongside a well-known technology company's name turns up numerous public write-ups; Homework Problem 4B and Challenge 2's tone requirements both benefit from reading two or three of these before writing your own.
- **Real DoD checklists** — many public open-source project contribution guides (`CONTRIBUTING.md` files on GitHub) publish an explicit "what counts as done" list for pull requests; comparing several against Lecture 1's four-category structure is good extra practice before Exercise 1.

## Deeper background (optional this week)

- **PMI — "Quality Management" knowledge area (PMBOK):** <https://www.pmi.org/pmbok-guide-standards>
  *Why: the fuller, formal treatment of quality planning and quality control behind this week's lighter, practitioner-focused version.*
- **Google SRE Book — "Service Level Objectives":** <https://sre.google/sre-book/service-level-objectives/>
  *Why: how a team decides what threshold actually belongs in a rollback trigger condition (Lecture 3, §1), grounded in SLOs rather than intuition.*
- **ITIL / AXELOS — "Release and Deployment Management" overview:** <https://www.axelos.com/certifications/itil-service-management>
  *Why: the enterprise-ITSM framing of release management, useful if you'll work somewhere with a heavier change-management process than this week's lightweight version.*

## Tools referenced this week

- **PostgreSQL 16+** or **SQLite 3.35+** — the `atlas_pm` database extended this week. See [Week 6's resources.md](../week-06-risk-and-dependency-management/resources.md) for install steps if you don't have it yet.
- **Python 3.10+ with pandas** (`pip install pandas`) — used lightly this week for the timeline-duration calculations in Challenge 2; see [Week 8's resources.md](../week-08-delivery-metrics-and-flow/resources.md) for a from-scratch pandas setup.
- No new tools to install this week — everything runs on the same database and SQL/Python setup from Weeks 6–9.

## Glossary

| Term | Definition |
|------|------------|
| **Definition of done (DoD)** | A fixed checklist, spanning code/tests/docs/sign-off, that applies to every piece of work of a given kind — the floor beneath acceptance criteria. |
| **Quality gate** | A checkpoint where work cannot proceed until specific criteria are met; valuable only when failing it produces real, honest signal. |
| **Release readiness checklist** | A broader, event-level checklist covering environment, data, monitoring, comms, and support — everything true only on release day, beyond the DoD. |
| **Code freeze** | The point at which a specific commit is tagged as the release candidate and further changes require explicit exception sign-off. |
| **Staged rollout** | Exposing a change to a small slice of users first, then widening it in stages if nothing goes wrong (canary, percentage-based, blue-green, dark launch). |
| **Big-bang release** | Releasing a change to 100% of users at once. |
| **Feature flag** | A runtime switch that decouples "code is deployed" from "code is live," enabling staged exposure and fast rollback. |
| **Go/no-go decision** | A scheduled, explicit decision — go, no-go, or conditional go — made by named owners reviewing the release's actual current state. |
| **Conditional go** | A go decision narrowed by a precisely stated condition, specific enough to act on without a follow-up question. |
| **Rollback / backout plan** | A pre-written plan with observable trigger conditions, ordered steps, named owners, and verification, designed before a release ships. |
| **Expand/contract** | A migration discipline: ship additive ("expand") schema changes first, defer destructive ("contract") changes to a later release once rollback is no longer needed. |
| **Post-release review (postmortem)** | A structured, blameless review that reconstructs a timeline, asks "why" toward a systemic cause, and produces owned, dated backlog action items. |
| **Blameless** | Assuming each person made a reasonable decision given what they knew at the time, and looking for the systemic gap rather than the person to fault. |
| **5 whys** | A technique for repeatedly asking "why" along one causal thread until reaching a systemic root cause rather than stopping at the first symptom. |

---

*Broken link? Open an issue or PR.*
