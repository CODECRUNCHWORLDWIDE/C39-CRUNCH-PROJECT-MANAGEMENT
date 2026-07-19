# Exercise 2 — Spot the Lifecycle Phase

**Goal:** Practice recognizing which of the five lifecycle phases (Lecture 2) a real event belongs to — the skill that lets you diagnose "what's actually going on" on any project, at any time.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 2, §1–6 before starting. Create a file `phases.md` in your portfolio at `c39-week-01/exercise-02/`.

## Tasks

Below are 12 events from Project Atlas (and a few from other Northlight initiatives), out of order. For each, name the lifecycle phase it belongs to — **Initiation, Planning, Execution, Monitoring & Control, or Closure** — and write one sentence explaining the tell (the specific thing in the event that gives it away).

1. Priya reviews and signs the Project Atlas charter, formally authorizing the team to staff against it.
2. Marcus's team discovers in week 4 that the third-party sharing API's sandbox environment is unreliable, and the team logs a new entry in the risk register.
3. Elena and Marcus break "Team Workspaces" into 18 backlog stories and Marcus's team sizes each one.
4. An engineer opens a pull request implementing the "invite teammates by email" story.
5. Priya and Elena formally confirm, against the charter's success criteria, that the shipped feature meets what was promised.
6. The PM notices the team is landing 3 stories per sprint against a plan that assumed 5, and flags it as a variance the group needs to address now.
7. The team runs a retrospective after launch and identifies that the third-party API risk should have been pressure-tested earlier.
8. The PM builds a 6-sprint delivery plan sequencing the backlog against the team's estimated capacity.
9. The PM sends Priya a weekly async status update: what shipped, what's next, what's at risk.
10. The team documents ongoing support/on-call ownership and hands Team Workspaces off to the product team as it moves from project to product.
11. Elena decides, based on a customer conversation, that commenting should ship before real-time presence indicators, and this gets reflected in the backlog order.
12. The PM and Elena agree to explicitly cut real-time presence indicators from the release to protect the launch date, and log the change.

## Expected result (spot checks)

- #1 → **Initiation** (charter sign-off is the defining artifact of this phase).
- #3, #8 → **Planning** (backlog breakdown and delivery-plan sequencing happen here).
- #4 → **Execution** (the actual work of building).
- #2, #6, #9, #12 → **Monitoring & Control** (risk discovery, variance detection, status reporting, and an approved scope change are all monitoring activities — note that #2 and #12 both touch the risk register but at different moments: logging a new risk vs. acting on one that materialized).
- #5, #7, #10 → **Closure** (formal acceptance, retrospective, handover).
- #11 → **Execution** (ongoing backlog reprioritization within an Agile sprint is part of the execution loop, not a planning-phase, once-only event — see Lecture 2 §1 on how planning/execution/monitoring repeat in miniature inside Agile).

## Done when…

- [ ] `phases.md` has all 12 events classified with a one-sentence tell for each.
- [ ] You can explain, in your own words, why #2 and #12 are both "Monitoring & Control" even though one is discovering a risk and the other is deciding a response to it.
- [ ] Your answers match the spot checks. For any mismatch, re-read Lecture 2 and rewrite — don't just flip the label.

## Stretch

Pick any one event above and rewrite it as it would look if the *previous* phase had been skipped (e.g., what would event #4 look like if planning — event #3 — had never happened?). Two to three sentences.

## Submission

Commit `phases.md` to your portfolio under `c39-week-01/exercise-02/`.
