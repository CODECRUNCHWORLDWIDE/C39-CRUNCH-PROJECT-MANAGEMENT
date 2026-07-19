# Exercise 2 — Assign Risk Responses

**Goal:** Practice choosing avoid/mitigate/transfer/accept (Lecture 1, §4) deliberately, with a stated reason — not defaulting to "mitigate" for everything.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 1, §4 (the four response types and the "mitigate as unstated accept" trap). You'll continue working with the Meridian risks from Exercise 1 — if you haven't finished Exercise 1, do that first; this exercise assumes your 12 scored risks already exist in the `risks` table. Create a file `risk-responses.sql` in your portfolio at `c39-week-06/exercise-02/`.

## Tasks

For **each** of the 12 Meridian risks from Exercise 1:

1. Assign exactly one `response_type`: `avoid`, `mitigate`, `transfer`, or `accept`.
2. Write a concrete `response_plan` — a real action, not a vague intention. "We'll keep an eye on it" is not a plan; "add a second engineer to shadow the migration script by [date] so no single point of failure exists on cutover day" is a plan.
3. Update each row in your `risks` table with an `UPDATE` statement setting `response_type` and `response_plan`.

Then answer, in comments at the top of your SQL file:

- Which risk (if any) genuinely calls for **avoid** — changing the plan so the risk can't happen at all? Not every register has one; say so if none does, and explain why.
- Which risk is the clearest candidate for **transfer**, and to whom/what (a vendor SLA, a contract clause, insurance)?
- Which risk did you almost mark "mitigate" by default, and what made you reconsider (or confirm) that choice?

## Expected result (spot checks)

There's real room for judgment here (that's the point), but a few should be unambiguous if you've internalized Lecture 1 §4:

- **#3 (PCI-DSS deadline)** should **not** be `accept` — the impact is severe (legally blocked from processing payments) and there's a real action available (confirm audit readiness now, escalate internally if behind schedule). `mitigate` (start the re-attestation process immediately, track weekly) is the strongest defensible answer; `avoid` would require restructuring the timeline so the deadline can't be missed, which may or may not be realistic — argue either way, but not `accept`.
- **#9 (theoretical future regulatory change, no signal)** is a reasonable `accept` — low probability, no concrete action available yet beyond noting it, and a mitigation plan would be speculative rather than concrete.
- **#5 (logging vendor renewal, audit-only impact)** is a reasonable candidate for **mitigate** (calendar the renewal date, assign an owner) or arguably **transfer** if the vendor offers a grace-period clause — either is defensible if you state the reasoning; `accept` alone, with no action, is weak given a low-cost fix (a calendar reminder) is available.
- **#2 (key-person risk on retry logic)** should have a concrete `mitigate` plan (pair programming, documentation, knowledge-share session) — `accept` here is the "default mitigate" trap in reverse: too important to wave off with nothing.

## Done when…

- [ ] All 12 risks have a `response_type` and a concrete `response_plan` (not a restated risk description).
- [ ] The three comment-block questions at the top of the file are answered.
- [ ] At least one risk is `accept` with a stated reason (not every risk should get an active plan — Exercise 2 is testing judgment about *which* ones, not universal action).
- [ ] No risk has `response_type = 'mitigate'` with a `response_plan` that's just a restatement of the risk ("mitigate the deployment risk" is not a plan).

## Stretch

For the risk you scored highest in Exercise 1, write the response plan as if you were presenting it to a skeptical sponsor in two sentences — the way Lecture 1's Atlas example was written for Priya. What's the one sentence that would make a sponsor trust you've actually thought it through, versus checked a box?

## Submission

Commit `risk-responses.sql` to your portfolio under `c39-week-06/exercise-02/`.
