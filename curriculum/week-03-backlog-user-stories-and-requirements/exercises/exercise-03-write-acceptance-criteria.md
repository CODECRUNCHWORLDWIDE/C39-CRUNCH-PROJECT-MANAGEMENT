# Exercise 3 — Add Given/When/Then Acceptance Criteria

**Goal:** Turn well-formed stories into unambiguous, testable acceptance criteria, including edge cases — the "Confirmation" step from Lecture 1's Three Cs.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 3 §2 (Given/When/Then) before starting. Create a file `acceptance-criteria.md` in your portfolio at `c39-week-03/exercise-03/`.

## The stories

Three ready, well-sliced Atlas stories need acceptance criteria before they can pass Definition of Ready (Lecture 3 §3) and enter a sprint:

1. *As an account admin, I want to archive a workspace, so that inactive team spaces stop cluttering the workspace list without permanently losing the data.*
2. *As a workspace member with view-only access, I want to be blocked from editing a shared dashboard's filters, so that I can't accidentally change what my teammates see.*
3. *As an account admin, I want to remove a teammate from a workspace, so that access is revoked the moment someone leaves the team.*

## Tasks

For **each** story, write:

1. **One happy-path scenario** in Given/When/Then form.
2. **At least two edge-case scenarios** — think about: What if the actor lacks permission? What if the target state is already what's being requested (e.g., archiving an already-archived workspace)? What if the action affects someone else who should be notified? What if input is invalid or missing?
3. **One sentence** explaining why each edge case matters — what would go wrong (a real bug, a confused user, a support ticket) if that scenario were never discussed before building.

Use the exact Given/When/Then format from Lecture 3 §2 — each "Then" must be **observable** (something a person or an automated test could actually check), not internal ("processed correctly," "handled properly" are not acceptable "Then" clauses — reread §2's rule on this before you start).

## Expected result (spot checks)

- **Story 1 (archive)** should surface at least: what happens to workspace *members'* access when it's archived (do they lose access immediately, or keep read access to old data?) and whether an already-archived workspace shows a different state if someone tries to archive it again.
- **Story 2 (blocked editing)** should surface at least: what the view-only member sees when they try to edit (silently nothing happens? a visible lock icon? an error?) and whether they can still do something else with the dashboard (view, comment) while blocked from editing filters.
- **Story 3 (remove teammate)** should surface at least: what happens to that person's existing comments/contributions after removal (deleted? kept but attributed?) and whether the removed person is notified.

If your scenarios don't touch at least one of these per story, you likely wrote the happy path only — go back and ask "what could go wrong or differ here?" per Lecture 3 §2.

## Done when…

- [ ] All 3 stories have 1 happy-path + 2+ edge-case scenarios in correct Given/When/Then form.
- [ ] Every "Then" clause is observable/testable, not internal.
- [ ] Each edge case has a one-sentence justification for why it matters.
- [ ] Your edge cases cover at least the spot-check items above for each story.

## Stretch

For story 1 (archive), imagine you're the engineer building it and the acceptance criteria above left one real question unanswered. Write the follow-up question you'd ask in refinement, and your best guess at the answer if Elena weren't available to ask right now — flagged clearly as an assumption, per Lecture 1 §4's point about elicitation.

## Submission

Commit `acceptance-criteria.md` to your portfolio under `c39-week-03/exercise-03/`.
