# Exercise 1 — Design a Sprint Cadence and Ceremony Schedule

**Goal:** Turn Lecture 2's ceremonies into an actual weekly calendar for Project Atlas — the kind you could paste into a team's shared calendar tomorrow.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 2, §2 (the sprint as a container) and §3 (the four ceremonies) before starting. Create a file `sprint-cadence.md` in your portfolio at `c39-week-02/exercise-01/`.

## Scenario

Atlas runs **two-week sprints**, confirmed in Week 1. The team:

- Marcus (tech lead) + 4 engineers (Dana, Sam, Wes, and one you may name)
- Elena (product owner) — available most of most days, but protects two mornings a week for uninterrupted backlog refinement
- Priya (sponsor) — wants visibility but explicitly does **not** want to sit through daily standups; a 15-minute weekly async-friendly touchpoint is enough for her
- The team is fully remote across three time zones: US Eastern, UK, and India (Priya, Elena, Marcus, Dana, Sam are US Eastern or UK-adjacent; Wes is India-based)

## Tasks

1. **Lay out a two-week sprint calendar** (10 working days) as a table or list. For each of the four ceremonies, specify:
   - Which day(s) it happens
   - Duration
   - Who attends (be specific — not just "the team")
   - What it must produce, in one sentence, referencing Lecture 2's "what it's FOR" language (a sprint goal, surfaced blockers, stakeholder feedback + updated backlog, or owned action items)
2. **Solve the time zone problem for daily standup.** Wes is roughly 10.5 hours ahead of US Eastern. Propose a specific time (in UTC, plus what local time that is for Wes and for the rest of the team) that's genuinely workable for everyone — not just convenient for the majority. If no single live time works well for everyone, propose a concrete alternative (e.g., an async written standup format) and explain what real synchronous conversation, if any, still needs to happen daily.
3. **Justify sprint length.** In 3–5 sentences, explain why two weeks fits Atlas better than a one-week sprint or a four-week sprint, referencing something specific about Atlas's work (story sizes, the pace at which Elena can supply well-refined backlog items, stakeholder rhythm).
4. **Handle Priya's constraint.** Design her weekly touchpoint separately from the standup — specify when it happens relative to the sprint (e.g., tied to sprint review, or a standalone midweek slot) and what three things it must always cover.

## Expected result (spot checks)

- Your calendar should show **sprint planning near the start** of the sprint (day 1) and **review + retro near the end** (day 10, or split across days 9–10), not scattered randomly.
- Daily standup should appear on **every working day** of the sprint, same time, ~15 minutes.
- Priya's touchpoint should be **separate from daily standup** — she explicitly opted out of daily attendance.
- Your time zone proposal should show real math (a UTC time converted correctly to both US Eastern/UK and India local time), not just "let's do 10am" with no conversion shown.

## Done when…

- [ ] `sprint-cadence.md` has a full 10-day calendar covering all four ceremonies with day, duration, attendees, and output for each.
- [ ] The time zone solution names a specific UTC time and shows the local-time conversion for at least Wes and one US/UK teammate.
- [ ] The sprint-length justification references a specific Atlas detail, not a generic "two weeks is standard" statement.
- [ ] Priya's touchpoint is scheduled, distinct from standup, with its three required topics named.

## Stretch

Marcus mentions that Wes has repeatedly said the standup time is workable but exhausting long-term (it's late evening for him every single day). Redesign the cadence to rotate the inconvenience fairly, or propose a structural fix (e.g., an alternating live/async pattern) — and explain the trade-off you're accepting either way.

## Submission

Commit `sprint-cadence.md` to your portfolio under `c39-week-02/exercise-01/`.
