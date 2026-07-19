# Exercise 3 — Re-Plan a Roadmap After a Two-Week Slip

**Goal:** Practice the full re-plan loop from Lecture 3: decide whether a slip warrants escalation, recompute remaining capacity, protect the fixed date, and write the early communication — the complete muscle, not just the math.

**Estimated time:** 60–90 minutes.

## Setup

Re-read Lecture 3 in full. Use the **original** (unmodified) `sprints` table from Lecture 2, §2 — 19 points every sprint, 114 total. Create a file `replan.md` in your portfolio at `c39-week-05/exercise-03/`.

## The scenario

This is the same slip Lecture 3 walks through (Advanced Permissions taking 2 sprints longer than planned, discovered at the Sprint 2 review) — but you are doing the analysis and the write-up yourself, from scratch, checking your reasoning against the lecture's afterward rather than reading the lecture's numbers first.

## Tasks

1. **Run the threshold test.** Using Lecture 3, §2's four-column table, write one sentence per column explaining why this specific slip (size, downstream dependents, external commitments, visibility already given) does or does not warrant a roadmap-level re-plan. State your conclusion.

2. **Recompute remaining capacity.** Using SQL or Python (your choice), compute: capacity consumed through Sprint 4 (hardening + Permissions + the 8-point rework overhead described in Lecture 3, §3), and capacity remaining for Sprints 5–6. Show your code and its output.

3. **Sequence the remaining backlog**, giving SOC 2 first claim on remaining capacity (it has the hard, immovable date), then Data Export API, then Onboarding revamp. Identify exactly where the plan runs out of room, and state the size of the gap in points.

4. **Make the scope call.** For whatever doesn't fit, decide explicitly: trim it, defer it whole to Q4, or something else — and justify the call in 2–3 sentences using the triple constraint (Week 1, Lecture 3, §3) and this week's Lecture 3, §4.

5. **Write the slip-communication message** to Priya, following the pattern in Lecture 3, §5: short version first, what stays fixed, what's still on track, what's at risk, and a clear (not buried) ask. Write your own message — do not copy the lecture's example verbatim; it should reflect your own scope-call decision from Task 4, which may differ from the lecture's.

6. **Log the change.** Write the two `INSERT` statements (or one, if you only made one distinct change) you'd add to `roadmap_change_log` (Lecture 3, §6) to record this slip and its resulting scope decision.

## Expected result (spot checks)

- Capacity consumed through Sprint 4: **46** points (8 + 30 + 8 rework overhead).
- Capacity remaining for Sprints 5–6: **68** points.
- SOC 2 (20) + full Data Export API (26) = 46, fits inside 68 with room to spare.
- Adding Onboarding revamp (24) brings the total to 70 — **2 points over** the 68 remaining.
- Your Task 1 conclusion should be: **yes, this slip warrants a re-plan** — it fails all four threshold-test columns, not just one.

## Done when…

- [ ] `replan.md` contains the threshold-test reasoning, the recomputed capacity (with code), the sequencing, your scope call with justification, the slip message, and the change-log entries.
- [ ] Your slip message names what's protected (SOC 2's date) and what's at risk (Onboarding) without burying either.
- [ ] Your scope call is a real decision (trim or defer), not a hope that the team "just runs faster."

## Stretch

Suppose Priya responds to your message and says the SOC 2 date is actually more flexible than you were told — the auditor can push fieldwork to October 1 if Northlight asks now. Redo the capacity math with this new fixed date and explain, in a short paragraph, what changes about your recommended scope call as a result.

## Submission

Commit `replan.md` to your portfolio under `c39-week-05/exercise-03/`.
