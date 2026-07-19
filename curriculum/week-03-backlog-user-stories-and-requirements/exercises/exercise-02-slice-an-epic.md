# Exercise 2 — Slice One Epic into Thin Vertical Stories

**Goal:** Practice vertical slicing (Lecture 2 §3–4) on a real epic, avoiding the horizontal-layer trap, and produce a set of stories that each pass INVEST.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 2 §3 (vertical slicing vs. the horizontal-layer trap) and §4 (slicing patterns) before starting. Create a file `sliced-epic.md` in your portfolio at `c39-week-03/exercise-02/`.

## The epic

Northlight's Atlas team has a new epic, drafted by Elena after the customer interviews mentioned in Lecture 1:

> **Epic: Dashboard Sharing.** Enterprise teams need to get a specific Insights dashboard in front of their whole workspace, with control over who can see it and whether it stays up to date, so they stop manually re-exporting and emailing screenshots every week.

Elena has scribbled a few notes from the customer interviews that hint at the real scope hiding inside this epic:

- Two of three interviewed customers want to share a dashboard as a **live, always-current view** other members can open directly in Insights.
- One customer specifically asked for a **point-in-time snapshot** (a PDF or static export) for external stakeholders who don't have Insights logins at all — a genuinely different need from "share it live."
- Every customer assumed sharing would default to **view-only**; none assumed edit access.
- One customer asked "can I un-share it later if I shared it with the wrong workspace?" — implying revocation is a real, separate need, not an afterthought.
- One customer mentioned dashboards with **filters applied** (e.g., "just Q3, just the EU region") and specifically wants the shared version to keep those filters, not reset to the default view.

## Tasks

1. **Do NOT slice this epic by technical layer** (schema story / API story / UI story) — that's the trap Lecture 2 §3 warns against. If you catch yourself writing a layer-named story, stop and re-read that section.
2. **Slice the epic into 5–8 thin, vertical stories**, each in proper role–goal–benefit form, using at least **three different slicing patterns** from Lecture 2 §4 (name which pattern you used for each story).
3. For each story, **run the INVEST checklist** in one short line per letter (or note "N/A — see story X" if it clearly inherits from a sequencing dependency, per the Independent example in Lecture 2 §2).
4. **Flag anything you deliberately left out** of this slice (e.g., "un-sharing" or "filtered snapshots") and say which slicing pattern justifies deferring it (see Lecture 2 §4's "defer the hard part" pattern) rather than silently dropping it.

## Expected result (spot checks)

A strong slice separates at minimum:

- Sharing a dashboard **live** (in-app, for existing Insights users) vs. sharing a **snapshot** (exported, for people without logins) — these are genuinely different capabilities (interview note #2), not the same story with a checkbox.
- Setting the **default access level** (view-only) as its own or bundled-but-explicit concern (interview note #3).
- **Revoking** a share as its own story (interview note #4) — easy to forget if you only think "forward" through the workflow.
- Whether **filters carry through** to the shared view (interview note #5) is either its own story or an explicit acceptance-criterion of the "share live" story — either is defensible, but it must be a *stated* decision, not silently missing.

If your slice has fewer than 5 stories, you likely bundled at least two of the above. If you have a story literally named "backend" or "database" anything, re-read Lecture 2 §3 before continuing.

## Done when…

- [ ] 5–8 stories, all in role–goal–benefit form, none named after a technical layer.
- [ ] At least three distinct slicing patterns used and labeled.
- [ ] INVEST run against every story.
- [ ] At least one deliberately deferred item, named and justified.

## Stretch

Story-map your slice: order the stories left-to-right as a user would experience them (the workflow-step pattern from Lecture 2 §4), and draw a horizontal line under the ones you'd include in a minimal first release (a rough MoSCoW "Must have" cut — Lecture 3 §4 goes deep on this next).

## Submission

Commit `sliced-epic.md` to your portfolio under `c39-week-03/exercise-02/`.
