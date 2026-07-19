# Exercise 1 — Build a Power/Interest Grid

**Goal:** Score a full stakeholder cast on power and interest, place each one in the correct quadrant, and write a one-line engagement strategy for each. By the end, quadrant placement feels like a reflex, not a guess.

**Estimated time:** 1 hour.

## Setup

Make sure `atlas_pm` has the `stakeholders` table from the [week README](../README.md). If Lecture 1's eight rows aren't inserted yet, insert them now — you'll need them for Task 1.

## Part A — Score a new cast (not the Atlas eight)

Northlight is starting a **second** initiative alongside Atlas: a small internal tool called **"Onboarding Copilot"** that helps new Northlight employees get set up (accounts, access, first-week checklist). Here's the cast, with a short description of each. You score them.

| Name | Role | Description |
|------|------|--------------|
| Rosa Delgado | VP People Ops | Requested the project; controls the People Ops budget line funding it |
| Chen Wei | IT Security Lead | Must approve any tool touching account provisioning; has vetoed two similar tools in the past two years |
| Marcus Diallo | Tech Lead (same Marcus from Atlas) | Asked to spend 20% of his time advising, informally, since he built Atlas's onboarding-adjacent auth code |
| New-hire cohort (represented by a rotating "New Hire Council" of 3 employees) | End users | Will actually use the tool daily in their first two weeks; no budget or veto power |
| Facilities Manager | Building/desk assignment | Owns one small piece of the onboarding checklist (desk assignment); otherwise uninvolved |
| CFO (Tom Reilly, same Tom from Atlas) | Approves any cross-department tool spend over $10k | Has not been briefed on this project yet |

## Tasks

Create `stakeholder-grid.md` in your `exercise-01/` folder and complete these:

1. **Score each of the six rows** above on power (1–5) and interest (1–5), with a one-sentence justification for each score — same discipline as Lecture 1, §2. Don't just copy the Atlas scores; think about *this* project's specifics (e.g., Chen Wei's veto history is a strong power signal).

2. **Place each stakeholder in a quadrant** (Manage Closely / Keep Satisfied / Keep Informed / Monitor) and justify the placement in one sentence, referencing the definitions in Lecture 1, §3.

3. **Flag any silent blocker risk.** Using the same rule as Lecture 1 (power ≥ 4, interest ≤ 2), does anyone on this list qualify? If so, explain specifically what could go wrong if they're neglected — be concrete about the failure mode, not just "they might be upset."

4. **Write one engagement-strategy sentence per stakeholder** — channel, cadence, and message focus, in the style of Lecture 1, §6. (Full communication-plan rows come in Exercise 2/the mini-project; here, one sentence each is enough.)

5. **Insert your six rows into `stakeholders`** using the same `INSERT` pattern as Lecture 1, giving each a unique `stakeholder_id` (start at 9, continuing from the Atlas cast) and `org_unit = 'Onboarding Copilot'` so you can distinguish the two projects in one table.

## Part B — Query it

Write and run these three queries against your combined table (Atlas + Onboarding Copilot), saving them in `grid-queries.sql`:

6. Every stakeholder across both projects in the Manage Closely quadrant, sorted by power descending.
7. Every stakeholder flagged as a silent blocker risk, with their org unit.
8. A count of stakeholders per quadrant, per project (`org_unit`) — a `GROUP BY` on two columns.

## Expected result (spot checks)

- Chen Wei should land power ≥ 4 (real veto authority) — if you scored her below 3, re-read Lecture 1 §2's definition of power (can this person block or kill the effort, not just "seems senior").
- The New Hire Council should land in Keep Informed or Monitor, not Manage Closely — they have real interest but genuinely no formal power over the project's direction.
- At least one row on your list should qualify as a silent blocker risk (Task 3) — if none do, double-check your power/interest scores against the actual descriptions given, not a gut feeling about seniority.

## Done when…

- [ ] All 6 new stakeholders are scored, justified, quadrant-placed, and inserted into `stakeholders`.
- [ ] Every quadrant placement has a one-sentence justification tied to the actual power/interest scores, not just asserted.
- [ ] At least one silent blocker is correctly identified with a concrete failure scenario described.
- [ ] All three queries in `grid-queries.sql` run without error and return sensible results.

## Stretch

- Draw the grid as an actual 2×2 ASCII or Markdown table (like Lecture 1, §3) with all 6 new stakeholders placed by name, not just listed in prose.
- For Chen Wei specifically, write the one-paragraph engagement strategy you'd actually use given her veto history — how would you engage her *earlier* than a typical Keep-Satisfied stakeholder, and why does her history change your approach?

## Submission

Commit `stakeholder-grid.md` and `grid-queries.sql` to your portfolio under `c39-week-07/exercise-01/`.
