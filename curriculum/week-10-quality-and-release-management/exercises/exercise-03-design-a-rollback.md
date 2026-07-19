# Exercise 3 — Design a Rollback Plan for a Release

**Goal:** Design a full rollback plan for Onboarding Copilot v1 — trigger conditions, ordered steps, named owners, time estimates, and verification — written before the release, the way Lecture 3 insists it has to be. By the end, you can write a rollback plan for a release that has no feature flag and no staged rollout to fall back on, which is the harder, more common case.

**Estimated time:** 1 hour. **References:** [Lecture 3, §1–2](../lecture-notes/03-rollback-and-postmortem.md).

## Setup

You need the `rollback_steps` table from the [week README](../README.md), and it helps to have your Exercise 2 checklist in front of you (rollback readiness is one of its categories).

## The harder case

Atlas's rollback plan leaned heavily on one thing: a feature flag that could be flipped off in seconds, decoupled from any deploy. **Onboarding Copilot v1 has no feature flag** — it's a small internal tool, and the team decided a flag was overkill for something this size. That decision is defensible (Lecture 2, §4's "big-bang is defensible when the change is small, low-risk, and well-tested" applies here) — but it means your rollback plan has to work **without** the fast lever Atlas had. That's the actual skill this exercise is testing: rollback design when the cheap, fast option isn't available.

## Tasks

In `rollback-plan.md`:

1. **Write 2–3 trigger conditions**, each observable and specific (Lecture 3, §1) — not "if it seems broken." Think concretely about what actually breaks for Onboarding Copilot: a new hire's accounts not being created correctly is the headline risk, so at least one trigger should name a specific, measurable version of that (e.g., "more than 2 of the last 10 new-hire provisioning runs fail to create all three accounts").
2. **Write 4–6 ordered rollback steps**, each with an owner, a time estimate, and a verification step (Lecture 3, §2's table format) — since there's no flag to flip, your first real step is something else. What is the actual fastest safe action here? (Consider: can new provisioning be paused for new hires starting today while a fix is written, even without a flag? Is that different from a "rollback" in the code sense?)
3. **Name who has authority to pull each trigger**, and justify why, the same way Lecture 3, §2 explained why Omar Farouk alone can flip Atlas's flag but Marcus Diallo has to sign off on redeploying. Given Onboarding Copilot's cast, who's the equivalent low-cost, fast-authority person, and who's the equivalent higher-cost, higher-authority sign-off?
4. **Address the account-provisioning-specific risk directly**: if accounts were partially created before the rollback trigger fired, what happens to those partial accounts? Leaving orphaned accounts behind is itself a security risk Chen Wei would flag — your plan needs a step for cleaning them up, not just stopping new ones.

## Load it

Insert your steps into `rollback_steps` with `release_name = 'onboarding-copilot-v1'`, continuing the `step_id` sequence from Lecture 3's Atlas rows (start at 7), numbering your own `step_no` starting at 1.

## Query it

Save in `rollback-queries.sql`, with output pasted into `rollback-plan.md`:

5. All rollback steps for Onboarding Copilot v1, in `step_no` order, with total estimated minutes summed across the whole plan. *(Hint: `SUM(est_minutes)`.)*
6. A single query across both releases comparing total rollback time (`SUM(est_minutes)` `GROUP BY release_name`) — is Onboarding Copilot's rollback actually faster or slower than Atlas's, and does that match what you'd expect given it has no flag?

## Expected result (spot checks)

- You have at least one trigger condition and at least one step directly addressing partial/orphaned account cleanup.
- Query 6 likely shows Onboarding Copilot's total rollback time is **not** dramatically faster than Atlas's despite being a "smaller" release — the missing feature flag removes the cheapest, fastest lever, which is exactly the point of this exercise.

## Done when…

- [ ] `rollback-plan.md` has 2–3 observable trigger conditions and 4–6 ordered steps with owners, time estimates, and verification.
- [ ] The plan explicitly addresses orphaned/partial-account cleanup.
- [ ] Trigger authority is assigned and justified for at least two different steps at two different cost/risk levels.
- [ ] All rows loaded into `rollback_steps` with the correct `release_name`; both queries run and are pasted into the deliverable.

## Stretch

- Given what this exercise revealed about the cost of *not* having a feature flag, write two sentences: would you now recommend Onboarding Copilot add one before v1 ships, even though the team decided against it? Weigh the coordination cost you'd be adding against the rollback speed you'd gain.
- Write the exact Slack message (Lecture 3, §2, step 2's pattern) you'd post to notify People Ops and IT the moment a rollback trigger fires — real words, not a description.

## Submission

Commit `rollback-plan.md` and `rollback-queries.sql` to your portfolio under `c39-week-10/exercise-03/`.
