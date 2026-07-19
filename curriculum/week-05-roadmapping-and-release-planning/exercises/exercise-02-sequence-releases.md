# Exercise 2 — Sequence a Backlog into Three Releases

**Goal:** Practice release-grain sequencing — grouping a backlog into fixed-scope and fixed-date releases against real, *uneven* capacity, respecting dependencies. This exercise deliberately changes one input from Lecture 2's worked example, so you cannot just copy the lecture's release plan — you have to re-derive it.

**Estimated time:** 60–90 minutes.

## Setup

Re-read Lecture 2 in full before starting. Load `roadmap_items` and `sprints` from Lecture 2, §2. Create a file `release-plan.md` in your portfolio at `c39-week-05/exercise-02/`.

## The changed input

Marcus's team just confirmed a **company-wide offsite in Sprint 5** that will eat roughly half the sprint. Update your `sprints` table:

```sql
UPDATE sprints SET planned_capacity = 10 WHERE sprint_num = 5;
```

Sprint 5's capacity drops from 19 to **10**. Every other sprint stays at 19. Total Q3 capacity is no longer 114 — recompute it.

## Tasks

1. **Recompute total capacity.** Sum `planned_capacity` across all 6 sprints with the updated Sprint 5 value. Show the query and result.

2. **Re-run the release-grouping query.** Using the same running-sum approach as Lecture 2, §4–5, but grouping into releases by **actual cumulative sprint capacity** rather than a flat "38 points per release" assumption (that assumption no longer holds once Sprint 5 changes). Specifically: R1 = Sprints 1–2, R2 = Sprints 3–4, R3 = Sprints 5–6 — compute each release's *real* capacity from the `sprints` table rather than assuming 38 for all three.

3. **Sequence the backlog into R1/R2/R3,** respecting: (a) priority order, (b) the SOC 2 hard date (must be usable/stable by 2026-09-13 — i.e., must complete within R2 at the latest), (c) the dependency (item 3 cannot be scheduled in an earlier release than item 2). Decide explicitly what happens to Data Export API's JSON export piece and to Onboarding revamp, given R3's now-reduced capacity — you will need to make and **state** a scope call, the same way Lecture 2, §6 modeled trimming Data Export API for R2.

4. **Write the release plan** as a markdown table: release, sprint range, capacity, items included, items trimmed/deferred and why.

## Expected result (spot checks)

- Total Q3 capacity: **105** (19×5 + 10), not 114.
- R3's capacity is **29** (10 + 19), not 38.
- R1 and R2 should sequence identically to Lecture 2's worked example (items 1+2 in R1; items 3 + trimmed item 4 in R2) — the changed input only affects R3, since Sprint 5 falls inside R3.
- R3 now has only 29 points for Onboarding revamp (24) + the deferred JSON export slice (8) = 32 total demand against 29 available — **3 points over**. Your plan must explicitly resolve this 3-point gap (trim further, or defer the JSON export piece entirely into Q4) rather than ignore it.

## Done when…

- [ ] `release-plan.md` shows the recomputed total capacity and R3's specific capacity.
- [ ] Your release table clearly marks every trim/deferral with a one-sentence reason.
- [ ] You can explain, in one paragraph, why R1 and R2 are unaffected by the Sprint 5 change while R3 is — tying it to which sprints fall inside which release.

## Stretch

Suppose instead of an offsite, Sprint 5's reduced capacity were caused by two engineers being pulled onto an unplanned production incident for the whole sprint. Does that change *how* you'd communicate the R3 trim to Priya, even though the numbers are identical? Write 3–4 sentences — this previews Lecture 3's distinction between a slip you control and one imposed from outside.

## Submission

Commit `release-plan.md` to your portfolio under `c39-week-05/exercise-02/`.
