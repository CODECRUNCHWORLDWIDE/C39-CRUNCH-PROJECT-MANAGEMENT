# Week 3 — Backlog, User Stories & Requirements

> **Goal:** by Sunday you can take a raw, messy feature brief and turn it into a prioritized, ready backlog — epics broken into thin vertical stories, each with a real role–goal–benefit and Given/When/Then acceptance criteria, ordered by a method you can defend out loud to a VP.

Week 1 gave you the charter — the scope and success criteria for a project. Week 2 (Agile, Scrum & Kanban) gave you the machinery that turns work into shipped increments, sprint by sprint. This week fills in the piece that connects them: **how a fuzzy stakeholder ask becomes the concrete, well-formed backlog items that machinery actually runs on.** Get this wrong and every ceremony downstream — sprint planning, standups, reviews — is running smoothly on the wrong work. Get it right, and "what should we build next" stops being a debate and starts being a defensible, revisitable decision.

We continue the running case study from Weeks 1–2: **Northlight Software** and **Project Atlas**, the "Team Workspaces" feature. This week, Priya's hallway comment about enterprise customers wanting team collaboration becomes real backlog items — you'll write the actual stories, slice a real epic, add acceptance criteria that remove ambiguity, and prioritize under the same renewal-deadline pressure the charter established in Week 1.

## Learning objectives

By the end of this week, you will be able to:

- **Elicit** requirements and needs from a stakeholder without turning them into a rigid, premature spec.
- **Write** user stories in role–goal–benefit form that carry real intent, not just a task description.
- **Apply** the INVEST checklist to test whether a story is genuinely ready to be pulled into a sprint.
- **Slice** epics into thin, vertically sliced stories that are each independently valuable and demoable — and explain why horizontal (technical-layer) slicing blocks real feedback.
- **Write** Given/When/Then acceptance criteria that make "done" unambiguous, including real edge cases.
- **Prioritize** a backlog using a defensible method — MoSCoW, value/effort, or WSJF — instead of gut feel or recency.

## Prerequisites

- Week 1 (charter, success criteria) and Week 2 (Agile/Scrum/Kanban roles and ceremonies) completed — this week assumes you know what a sprint and a backlog are, and builds directly on Project Atlas's charter.
- Comfortable reading a messy, informal brief and writing clear, structured Markdown — this week is conceptual and writing-heavy, one exercise (Exercise 1) is pure rewriting practice.
- A free [GitHub](https://github.com/join) account (from Week 1) — the mini-project's live board can be built in GitHub Projects, or in a free Jira account (see [resources.md](./resources.md)) if you prefer.
- Nothing new to install this week beyond optionally setting up a GitHub Projects board or free Jira account for the mini-project.

## How this week is organized

Elicitation feeds stories, stories get sliced thin, thin stories get acceptance criteria and a place in line. Work top to bottom — each piece assumes the ones above it.

| # | File | What's inside | ~Time |
|--:|------|---------------|------:|
| 1 | [lecture-notes/01-from-requirements-to-stories.md](./lecture-notes/01-from-requirements-to-stories.md) | The spec-vs-conversation trap, the Three Cs, role–goal–benefit, and eliciting real needs from a stakeholder. | 2h |
| 2 | [lecture-notes/02-slicing-and-invest.md](./lecture-notes/02-slicing-and-invest.md) | The backlog hierarchy, the INVEST checklist, vertical vs. horizontal slicing, and concrete splitting patterns. | 2h |
| 3 | [lecture-notes/03-acceptance-and-prioritization.md](./lecture-notes/03-acceptance-and-prioritization.md) | Given/When/Then acceptance criteria, Definition of Ready vs. Done, and MoSCoW/value-effort/WSJF prioritization. | 2h |
| 4 | [exercises/exercise-01-rewrite-bad-stories.md](./exercises/exercise-01-rewrite-bad-stories.md) | Rewrite eight weak, real-world-messy stories into INVEST-ready ones. | 1h |
| 5 | [exercises/exercise-02-slice-an-epic.md](./exercises/exercise-02-slice-an-epic.md) | Slice a real Atlas epic ("Dashboard Sharing") into thin vertical stories. | 1h |
| 6 | [exercises/exercise-03-write-acceptance-criteria.md](./exercises/exercise-03-write-acceptance-criteria.md) | Add Given/When/Then acceptance criteria, happy path plus edge cases, to three ready stories. | 1h |
| 7 | [challenges/challenge-01-backlog-from-a-brief.md](./challenges/challenge-01-backlog-from-a-brief.md) | Build a full backlog — epic, sliced stories, acceptance criteria, MoSCoW — from a raw, messy feature brief. | 1.5h |
| 8 | [challenges/challenge-02-prioritize-under-pressure.md](./challenges/challenge-02-prioritize-under-pressure.md) | Reprioritize that backlog under real pressure: an urgent new story and a scope trade-off, defended with WSJF. | 1h |
| 9 | [mini-project/README.md](./mini-project/README.md) | Turn a new feature brief into a sliced, prioritized, ready backlog — built as a real, live board. | 3h |
| 10 | [homework.md](./homework.md) | Extra practice, spread across the week. | 4h |
| 11 | [quiz.md](./quiz.md) | 15 self-check questions + answer key. | 1h |
| 12 | [resources.md](./resources.md) | Curated free/official reading + tools to install. | — |

## Weekly schedule

The schedule below adds up to approximately **17.5 hours** of scheduled work — a writing- and judgment-heavy week, with real tool time at the end for the mini-project's live board.

| Day | Focus | Lectures | Exercises | Challenges | Quiz/Read | Homework | Mini-Project | Daily Total |
|-----------|--------------------------------------------|---------:|----------:|-----------:|----------:|---------:|-------------:|------------:|
| Monday | Requirements → stories; Three Cs; role–goal–benefit | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Tuesday | INVEST + vertical slicing | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Wednesday | Acceptance criteria + prioritization methods | 2h | 1h | 0h | 0.5h | 1h | 0h | 4.5h |
| Thursday | Challenges: full backlog from a brief | 0h | 0h | 1.5h | 0.5h | 1h | 0h | 3h |
| Friday | Challenge: reprioritize under pressure; start mini-project | 0h | 0h | 1h | 0.5h | 1h | 1h | 3.5h |
| Saturday | Mini-project — backlog + live board | 0h | 0h | 0h | 0h | 0h | 2h | 2h |
| Sunday | Quiz + review | 0h | 0h | 0h | 1h | 0h | 0h | 1h |
| **Total** | | **6h** | **3h** | **2.5h** | **3.5h** | **4h** | **3h** | **~23.5h**† |

† Totals across the table are additive per column; the course's full-time pace budgets roughly 24 hrs/week, so treat the last row as a ceiling, not a requirement — most learners land closer to 16–18 hours actually spent, since reading and writing overlap.

## By the end of this week you can…

- Take a real, messy paragraph of stakeholder wants and turn it into an epic and a set of properly formed role–goal–benefit stories, catching hollow benefits and vague roles on sight.
- Slice any feature into thin, independently valuable, independently demoable stories — and explain, concretely, why slicing by technical layer instead breaks feedback and hides risk.
- Write Given/When/Then acceptance criteria precise enough that a PM and an engineer would agree, without a debate, on whether a story is actually done.
- Prioritize a backlog with MoSCoW for a fast stakeholder conversation, or WSJF when you need a rigorous, numbers-backed tiebreaker — and defend a reprioritization when an outside event changes what's urgent.

## Up next

Week 4 — Estimation & Sprint Planning — once the backlog is sliced, ready, and prioritized, the next question is how much of it actually fits in a sprint, and Week 4 turns your ready backlog into a real, capacity-checked delivery plan.

---

*Part of the Code Crunch Worldwide open curriculum · GPL-3.0 · If you find errors, please open an issue or PR.*
