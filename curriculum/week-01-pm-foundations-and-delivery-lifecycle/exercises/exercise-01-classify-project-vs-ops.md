# Exercise 1 — Classify: Project, Product, or Ops

**Goal:** Get the project/product/ops distinction (Lecture 1, §2) into your fingers, so you can classify real work on sight instead of hesitating.

**Estimated time:** 45–60 minutes.

## Setup

Re-read Lecture 1, §2 (the project/product/ops table and the trap paragraph) before starting. Create a file `classification.md` in your portfolio at `c39-week-01/exercise-01/`.

## Tasks

Below are 15 real work items from Northlight Software (some directly Atlas-related, some not). For **each one**, decide whether it's best classified as a **Project**, **Product**, or **Ops**, and write **one sentence** justifying the call using this lecture's definitions (temporary + defined end vs. ongoing evolution vs. repeatable/continuous). There is a correct answer for each — this exercise is testing recall and application, not opinion (Challenge 1 later this week gets into genuinely ambiguous cases).

1. Building and shipping the Team Workspaces feature (Project Atlas) by the Q3 deadline.
2. Northlight Insights as a whole — the analytics product the company sells.
3. Running the nightly ETL job that refreshes every customer's ad-spend data.
4. Migrating Northlight's database from an old provider to a new one over the next 8 weeks, with a fixed cutover date.
5. Triaging and responding to customer support tickets every day.
6. Continuously tuning the dashboard-rendering engine's performance as part of normal engineering work, with no fixed end date.
7. A one-time initiative to achieve SOC 2 compliance ahead of the Q3 audit.
8. The ongoing on-call rotation for production incidents.
9. Redesigning the pricing page for a specific launch event happening in six weeks.
10. Elena's continuous backlog grooming and prioritization for Northlight Insights.
11. A one-time data-cleanup effort to deduplicate 50,000 legacy customer records before a specific migration cutover.
12. The customer success team's quarterly business reviews with enterprise accounts.
13. Building a brand-new internal admin tool, from scratch, to launch in 10 weeks.
14. Maintaining and patching the admin tool once it's in production, indefinitely.
15. A hackathon-style 3-day internal effort to prototype a new charting feature, with a demo at the end and no commitment to ship it.

## Expected result (spot checks)

- #2, #6, #10, #14 → **Product** (or ongoing product-adjacent work with no defined end).
- #3, #5, #8, #12 → **Ops** (repeatable, continuous, no "done").
- #1, #4, #7, #9, #11, #13, #15 → **Project** (temporary, defined end, specific outcome — note #15 still counts even though it might not ship; it has a defined start, end, and deliverable: the demo).

## Done when…

- [ ] `classification.md` has all 15 items classified with a one-sentence justification each.
- [ ] Every justification references *why* (temporary/defined end vs. ongoing vs. repeatable), not just a label.
- [ ] Your answers match the spot checks above. If any don't match, re-read Lecture 1 §2 and rewrite your justification — don't just change the label.

## Stretch

Pick one item you classified as **Ops** and describe a scenario where it would actually become a **Project** instead (e.g., "the nightly ETL job" becomes a project if it's being rebuilt from scratch with a fixed cutover date). What changed?

## Submission

Commit `classification.md` to your portfolio under `c39-week-01/exercise-01/`.
