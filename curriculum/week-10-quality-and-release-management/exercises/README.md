# Week 10 — Exercises

Three exercises, ~3 hours total. Work them in order — each one builds the next layer of the same release, and Exercise 3 assumes the checklist from Exercise 2 exists.

## The practice project

The lectures ran entirely on Atlas's real GA (`atlas-ga-2026-04-29`). These exercises deliberately use a **different, smaller release** so you're not just copying Atlas's numbers — you're applying the same discipline to a fresh case with different stakes.

Northlight's second initiative, **Onboarding Copilot** (introduced in Week 7's Exercise 1 — the internal tool that helps new employees get set up), is about to ship its own **v1 release**. It's smaller than Atlas, has a much smaller blast radius if something goes wrong (internal employees only, not paying customers), and has a different, sharper risk: **Chen Wei, the IT Security Lead, has vetoed two similar tools in the past two years** because they touched account provisioning carelessly. If your DoD, checklist, and rollback plan don't take her seriously, you haven't actually learned this week's lesson — you've just copied a template.

The cast, recapped from Week 7:

| Name | Role |
|------|------|
| Rosa Delgado | VP People Ops — requested the project, funds it, is the sponsor |
| Chen Wei | IT Security Lead — must approve anything touching account provisioning |
| Marcus Diallo | Advising 20% of his time (same Marcus from Atlas) — built the onboarding-adjacent auth code |
| New Hire Council | 3 rotating employees — the actual end users |
| Facilities Manager | Owns desk assignment, one small piece of the onboarding checklist |
| Tom Reilly | CFO — must approve cross-department tool spend over $10k (same Tom from Atlas) |

Use `release_name = 'onboarding-copilot-v1'` for every row you insert this week, so it sits alongside `atlas-ga-2026-04-29` in the same tables without colliding.

## How to work these

1. Read the referenced lecture section before starting — each exercise cites exactly which one.
2. Write real SQL against your `atlas_pm` database. Don't just describe what you'd insert — actually run it and paste the query results into your deliverable.
3. Match the *rigor* of the lecture's Atlas example, not its length — Onboarding Copilot is a smaller release and should have a proportionally smaller DoD, checklist, and rollback plan, not a padded copy of Atlas's.
4. Every exercise ends with a "Done when…" checklist. Don't move to the next one until you can tick every box.

## Time budget

| Exercise | Time |
|---:|---:|
| 1 — Write a DoD | 1h |
| 2 — Build a release checklist | 1h |
| 3 — Design a rollback | 1h |
| **Total** | **~3h** |

When you finish all three, move on to the [challenges](../challenges/README.md).
