# Exercise 3 — Turn Vague Goals into Measurable Success Criteria

**Goal:** Practice the single hardest skill from Lecture 3 — converting a fuzzy aspiration into a success criterion that has a number, a timeframe, and can be checked by someone who wasn't in the room.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 3, §2 (the "success criteria" section and its test: "could two reasonable people, six months from now, agree on whether it was met?"). Create a file `success-criteria.md` in your portfolio at `c39-week-01/exercise-03/`.

## Tasks

Below are 8 vague goals, the kind a sponsor or stakeholder actually says out loud. For each, write **one rewritten success criterion** that passes the test from Lecture 3, plus **one sentence** naming what assumption you had to make to add the missing number/timeframe (since the vague version never specified it — a real PM would confirm the assumption with the stakeholder, but for this exercise, state it explicitly and move on).

1. "Customers love the new collaboration features."
2. "The migration should go smoothly."
3. "We want the app to feel fast."
4. "Support tickets should go down after this launch."
5. "The onboarding flow needs to be better."
6. "We want good adoption of the new feature."
7. "The system needs to be reliable."
8. "Make sure the launch doesn't hurt the business."

## Worked example (do it like this)

**Vague:** "The migration should go smoothly."

**Rewritten:** "The database migration completes with zero data loss (verified by row-count and checksum reconciliation pre/post-migration), less than 15 minutes of customer-facing downtime, and no P1 incidents in the 72 hours following cutover."

**Assumption stated:** I assumed "smoothly" means no data loss and minimal downtime specifically, since that's the two things a migration typically risks — I'd confirm with the sponsor whether a longer downtime window is actually acceptable in exchange for a safer migration approach.

## Expected result (spot checks)

Your rewrites should each contain: a **number** (a percentage, a count, a time duration, a threshold), a **timeframe** (by when, or over what window), and a **verification method** (how anyone would actually check it — a query, a survey, a dashboard, a report). If any of your 8 rewrites is missing one of these three elements, it will fail Lecture 3's test — go back and add it.

## Done when…

- [ ] `success-criteria.md` has all 8 goals rewritten with number + timeframe + verification method.
- [ ] Each rewrite has a one-sentence stated assumption.
- [ ] You can explain, for at least one rewrite, why the *original* vague version could never actually be used to make a decision.

## Stretch

Take success criterion #6 ("good adoption") and write **two different reasonable rewrites** reflecting two different stakeholder priorities (e.g., a growth-focused version emphasizing raw adoption rate vs. a retention-focused version emphasizing repeat usage). This mirrors real PM work: the "right" number often depends on whose goals you're serving, and naming that explicitly is part of the job.

## Submission

Commit `success-criteria.md` to your portfolio under `c39-week-01/exercise-03/`.
