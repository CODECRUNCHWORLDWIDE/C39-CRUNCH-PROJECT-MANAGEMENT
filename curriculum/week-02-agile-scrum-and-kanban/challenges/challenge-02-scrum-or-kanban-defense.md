# Challenge 2 — Recommend Scrum or Kanban and Defend It

**Goal:** Practice Lecture 3 §6's fit criteria on three teams whose shapes deliberately don't match the tidy "feature team vs. support team" split Lecture 3 used — real teams are messier than the lecture's clean example, and this challenge is that mess.

**Estimated time:** 60 minutes.

## Setup

Re-read Lecture 3, §2–6 in full before starting. Create a file `recommendation.md` in your portfolio at `c39-week-02/challenge-02/`.

## The three teams

### Team A — Northlight's Billing Integrations team

Three engineers who build and maintain integrations between Northlight Insights and customers' billing systems (Stripe, NetSuite, a few bespoke enterprise ones). About 70% of their work is planned: a sales team closes a new enterprise deal that requires a new billing integration, and that's known weeks in advance with a real (if sometimes shifting) target date tied to the deal's close. The other 30% is unplanned: an existing integration breaks because a third party changed their API without warning, and that has to be dropped everything and fixed, usually within a day. The team has one Slack channel where both kinds of requests land, currently with no distinction between them.

### Team B — Northlight's Design Systems team

Two designers and one front-end engineer who maintain Northlight's shared component library used across the whole product. Work arrives from many other teams requesting a new component or a change to an existing one, with no single prioritizer — currently whoever asks loudest or most recently tends to get worked on next, which the team itself describes as "exhausting and political." There's no fixed deadline pressure from any single request, but there is real frustration company-wide about how long requests take and how unpredictable turnaround is. The team has never used a formal framework at all — no board, no ceremonies, just a shared doc.

### Team C — Northlight's Data Platform team

Four engineers responsible for a multi-month migration off an aging data warehouse (a real project, chartered, with a hard date six months out, broken into a known sequence of large phases) *and* for keeping the current warehouse's nightly pipelines running, which occasionally fail overnight and need same-day triage before the next business day's dashboards are due. The migration is the team's official top priority; pipeline failures are rare (roughly once every two weeks) but always urgent when they happen.

## Tasks

For **each** team:

1. **Recommend Scrum, Kanban, or a Scrum/Kanban hybrid** ("Scrumban" — Lecture 3 §6's closing paragraph). Name it clearly.
2. **Defend the recommendation in 4–6 sentences**, explicitly using at least three of Lecture 3 §6's comparison dimensions (how work arrives, team's goal shape, stakeholder rhythm, change tolerance, how progress is measured) with specifics from that team's description — not generic Agile language that could apply to any team.
3. **Name the one thing that would break your recommendation** if it were true — a specific fact about the team that, if it changed, would flip your answer to a different framework. (This tests whether your reasoning is actually tied to evidence or is just a default answer.)

Then, in a short closing section:

4. **Compare Team A and Team C.** Both mix planned and unplanned work, but Lecture 3 might point you toward different answers for each (or the same answer, defended differently). Explain, in 3–4 sentences, what's actually different about *how* the unplanned work shows up in each team that does or doesn't change the recommendation.

## What a strong answer contains

- Three distinct, evidence-based recommendations — not the same answer for all three with search-and-replaced team names.
- Real engagement with the fact that Team A and Team C both have a planned/unplanned mix, without collapsing into "they're both the same, just do Scrumban for everything." The frequency, urgency, and *proportion* of unplanned work differs meaningfully between them — a strong answer notices and uses that difference.
- For Team B specifically: a recommendation that addresses the *stated* problem (unpredictable, political prioritization) — a framework choice that doesn't touch prioritization at all hasn't actually solved what Team B described as broken.
- The "what would break this" answer for each team should be a real, specific fact, not a throwaway ("if things changed" is not an answer).

## Submission

Commit `recommendation.md` to your portfolio under `c39-week-02/challenge-02/`.
