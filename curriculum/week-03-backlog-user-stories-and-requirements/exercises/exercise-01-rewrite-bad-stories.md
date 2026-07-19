# Exercise 1 — Rewrite Weak Stories into INVEST-Ready Ones

**Goal:** Recognize the anti-patterns from Lecture 1 §5 on sight, and rewrite each one into a proper role–goal–benefit story that would pass the INVEST checklist from Lecture 2 §2.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 1 §3 (the role–goal–benefit template) and §5 (anti-patterns) before starting. Create a file `rewrites.md` in your portfolio at `c39-week-03/exercise-01/`.

## Tasks

Below are eight real backlog items pulled from an early, messy draft of the Atlas backlog — each one has something wrong with it. For **each item**:

1. **Name the anti-pattern(s)** present (technical story in disguise, compound story, solution disguised as need, vague role, no/hollow benefit — an item may have more than one).
2. **Rewrite it** as a proper role–goal–benefit story (or, if it's genuinely not a user story — e.g., pure technical work — say so explicitly and write it as a clearly labeled technical/enabler item instead, don't force it into a costume).
3. **State one assumption** you made to fill in the missing detail (a real conversation would have resolved this — you're standing in for that conversation).

The eight items:

1. *"As a developer, I want to add caching to the workspace API, so that it's cleaner."*
2. *"As a user, I want workspace stuff to work better."*
3. *"As an admin, I want to create, rename, archive, and permanently delete workspaces, and manage all member permissions."*
4. *"Add a three-dot menu with Rename, Archive, and Delete options to each workspace card."*
5. *"As a workspace member, I want to comment on a dashboard, so that I can comment on a dashboard."*
6. *"As a user, I want notifications."*
7. *"As an account admin, I want to bulk-invite 50 teammates from a CSV upload, so that large enterprise teams can be set up in one action instead of one invite at a time."* *(Trick item — read carefully before deciding what, if anything, is actually wrong with this one.)*
8. *"As a workspace member, I want the app to be fast."*

## Expected result (spot checks)

- **#1** — technical story in disguise; no end-user benefit stated. Either reframe around the *user-facing* symptom it fixes (e.g., "As a workspace member, I want the shared dashboard to load in under 2 seconds, so that I'm not waiting every time I check it" — if that's the real motivation) or leave it as a labeled technical/enabler item, not a fake user story.
- **#2** — vague role ("user") and vague goal ("stuff," "better") — not estimable, not testable. Needs a real conversation before it can be written at all.
- **#3** — compound story bundling at least five capabilities (create/rename/archive/delete/manage-permissions); split per Lecture 2 §5.
- **#4** — solution disguised as need; no role, no benefit, dictates UI. The underlying goal (probably) is "admin wants to manage a workspace's lifecycle" — write the goal, not the menu.
- **#5** — hollow/circular benefit; needs the real "so that."
- **#6** — vague on every axis (role, goal, benefit) — this is a *feature area*, not a story; needs elicitation before it's writable.
- **#7** — this one is actually reasonably well-formed (role, goal, and a real, specific benefit)! It may still be too big to be *Small* (INVEST) depending on team capacity, worth flagging, but it isn't a bad story in the way 1–6 and 8 are — say so if you conclude that.
- **#8** — no measurable definition of "fast" — not Testable until quantified (compare to Lecture 3's point about observable "Then" clauses).

## Done when…

- [ ] All 8 items have a named anti-pattern (or, for #7, a note that it's actually well-formed).
- [ ] All items that should be user stories are rewritten in proper role–goal–benefit form.
- [ ] Each rewrite states the one assumption you made.
- [ ] Your analysis of #7 matches the spot check — if it doesn't, re-read Lecture 1 §3 and reconsider what makes a benefit "real" vs. "hollow."

## Stretch

Pick one rewritten story and run it through the full INVEST checklist (Lecture 2 §2), one line per letter, stating pass/fail and why.

## Submission

Commit `rewrites.md` to your portfolio under `c39-week-03/exercise-01/`.
