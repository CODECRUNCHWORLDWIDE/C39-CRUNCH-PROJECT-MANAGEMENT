# Mini-Project — A Quarterly Roadmap and Release Plan You Could Defend to a Sponsor

> Pick a real initiative — your own idea, something at work, or one of the provided briefs — and build a full quarterly roadmap and release plan for it: outcome-based, capacity-aware, dependency-sequenced, backed by real SQL/Python, and defensible to a skeptical sponsor. This is the week's capstone: everything from Lectures 1–3 lands in one connected set of artifacts.

**Estimated time:** 3–3.5 hours, best done Saturday after the exercises and challenges.

A roadmap built for Northlight is good rehearsal. A roadmap built for something you actually care about, backed by numbers you actually computed rather than eyeballed, is proof you can do this job for real. That's this mini-project.

---

## Step 1 — Choose your initiative and define a backlog

Pick **one**:

- **(A) Your own real idea.** A side project, a feature roadmap for something you're already building, an initiative at work. Strongest option if you have one.
- **(B) One of the provided briefs**, if you don't have a real one in mind:
  1. **"Study Buddy Matcher" — Q1 roadmap.** Themes: core matching (MVP), trust & safety (reporting/blocking), growth (referrals, campus partnerships), and a compliance theme (student-data privacy review before any campus rollout — has a real external deadline: the target campus's privacy office reviews new tools once per semester, and the next window closes in 10 weeks).
  2. **"Shift Swap" — Q1 roadmap.** Themes: core swapping (MVP), manager oversight & approvals, payroll-system integration (a partner dependency — the payroll vendor's API sandbox is only available starting week 4), and mobile notifications.
  3. **"Community Tool Library" — Q1 roadmap.** Themes: core listing & reservation (MVP), trust & safety (damage/loss reporting), neighborhood expansion (a growth theme competing for the same capacity), and a liability-insurance compliance theme with a hard signup deadline from the insurer.

Whichever you pick, write one sentence at the top of your deliverable naming your choice.

Then build a **backlog of at least 7 items** across **at least 3 themes**, each with: a title, a theme, an outcome statement (not a feature name — Lecture 1, §3), a story-point estimate, a priority rank, and a dependency where a real one exists. At least one item must have a **hard external date** you define and justify (a compliance deadline, a partner's API availability window, a seasonal/enrollment deadline — something genuinely outside your control).

---

## Step 2 — Build the database and run the sequencing

Following Lecture 2, §2–5 exactly:

1. Create `roadmap_items` and `sprints` tables (PostgreSQL or SQLite) reflecting your own backlog and your own assumed team capacity.
2. Derive your `sprints.planned_capacity` from a stated, justified velocity assumption (make up a plausible velocity history of 4 sprints, apply the same 15% buffer logic as Lecture 2, §3 — show the math, don't just assert a number).
3. Run the cumulative-sum query (Lecture 2, §4) against your backlog.
4. Write a dependency-integrity query (Lecture 2, §7 pattern) and confirm it returns zero violations against your final release assignments.

Save all SQL (schema, inserts, and the two queries above with their real output) in `roadmap.sql`.

---

## Step 3 — Write the now/next/later roadmap

In `roadmap.md`, produce the outcome-based now/next/later roadmap for your quarter, following Lecture 1, §5's format exactly (theme → outcome → initiative(s), per bucket). State your Now/Next/Later cutoffs using your own cumulative-points output from Step 2, not intuition.

---

## Step 4 — Write the release plan

In `release-plan.md`, produce a release-by-release plan (at least 2 releases) following Lecture 2's format: release name, sprint range, capacity, items included, and — critically — **at least one release must be explicitly labeled fixed-scope and at least one fixed-date**, with your fixed-date release built around the hard external date you defined in Step 1. If your hard-date item creates a capacity conflict (it should, if you sized it honestly), show the trim or trade-off you made to protect it, the way Lecture 2, §6 modeled for Data Export API.

---

## Step 5 — Stress-test it with a slip

In `replan.md`, invent a plausible slip (one item runs long — pick a specific, believable reason, not "it took longer than expected") and redo Lecture 3's full loop against your own plan: the threshold test (§2), the recomputed remaining capacity (§3, with real SQL/Python), the scope call (§4), and a short slip-communication message to your sponsor (§5).

---

## Deliverable

A directory in your portfolio `c39-week-05/mini-project/` containing:

1. `roadmap.sql` — schema, inserts, and both queries with real output (Step 2).
2. `roadmap.md` — the now/next/later roadmap (Step 3).
3. `release-plan.md` — the release plan with fixed-scope and fixed-date releases (Step 4).
4. `replan.md` — the slip stress-test (Step 5).

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Backlog realism | 15% | Outcome statements are genuine outcomes; the hard-date item is plausible and justified |
| SQL correctness | 20% | Schema and queries run as written; cumulative sums and dependency check are correct |
| Roadmap quality | 20% | Bucket cutoffs are derived from real numbers, not vibes; outcome language throughout |
| Release plan quality | 20% | At least one fixed-scope and one fixed-date release, with a real, reconciled trim shown |
| Re-plan quality | 15% | Threshold test is applied correctly; scope call is a real decision with real numbers |
| Communication | 10% | The slip message is early, small, and names what's protected vs. at risk clearly |

---

## Stretch goals

- Build the exec-vs-delivery presentation pair (Challenge 1's format) for your own roadmap and check them against each other for consistency.
- Add a second, independent hard-date scenario (Challenge 2's format) and show how you'd protect it.
- If you picked a real idea (Option A), share `roadmap.md` alone with someone unfamiliar with the project and ask them what's committed vs. directional — if they can't tell, revise the bucket language until they can.

---

## Why this matters

Every roadmap you'll ever build professionally has this exact shape: an outcome-based communication layer (Lecture 1) sitting on top of a capacity-and-dependency-correct release plan (Lecture 2), kept honest as reality intervenes (Lecture 3) — done with queryable data, not a spreadsheet nobody trusts by week 3. Keep these four files; Week 6 (risk & issue management) extends the same dataset with a risk register sitting alongside your `roadmap_items` table.

When done: push, then take the [quiz](../quiz.md).
