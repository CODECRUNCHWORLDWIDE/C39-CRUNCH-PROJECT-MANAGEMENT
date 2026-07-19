# Mini-Project — Stakeholder Map, Communication Plan & Status Pack

> Ship the three artifacts a real PM needs before a single status update goes out: a full stakeholder map with quadrant placement, a communication plan with per-audience cadence, and a sample status report — all backed by a live, queryable dataset, not a static document. This is the week's capstone: everything from Lectures 1–3 lands in one connected deliverable.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

A stakeholder map you draw once at kickoff and never open again is decoration. A stakeholder map that lives in a table you actually query before every update — "who's overdue," "who's a silent blocker I haven't touched in three weeks," "what did I tell Priya last time so I'm consistent this time" — is a working tool. That's what you're building this week: not a diagram, a system.

---

## Step 1 — Choose your project

Pick **one**:

- **(A) Project Atlas**, continuing the case study. Use the full cast from Lecture 1 (Priya, Elena, Marcus, Sofia, Derek, Amara, Tom, Jamie) plus at least **2 additional stakeholders you invent** who are realistic for a project like this (e.g., a Legal/Compliance reviewer gating the security review from Week 1's constraints, an Engineering Director above Marcus, a second at-risk account's champion). Inventing plausible additions and justifying them is part of the exercise.
- **(B) The same real project or provided brief you used in Week 1's mini-project** (your own idea, or one of "Study Buddy Matcher," "Shift Swap," "Community Tool Library"). Invent a realistic cast of 6–8 stakeholders for it, using the charter you already wrote in Week 1 to ground who'd actually have a stake.

Whichever you pick, name your choice in one sentence at the top of your deliverable.

---

## Step 2 — Build the stakeholder map (data, not a slide)

Set up (or extend, if using Atlas) the `stakeholders` table from the [week README](../README.md) in `atlas_pm` or your own project's database. Then:

1. **Insert every stakeholder** (6–10 total) with real power/interest scores (1–5 each) and a one-sentence justification for each score, written in `stakeholder-map.md` — not just in the database, since a reader needs your reasoning, not just your numbers.
2. **Assign each a quadrant** and update the `quadrant` column.
3. **Flag silent blocker risks** (power ≥ 4, interest ≤ 2) and write one paragraph on what could go wrong for each one if neglected — a *specific* failure scenario, not a generic warning.
4. **Query and paste the results** of at least two useful queries into `stakeholder-map.md` (e.g., everyone in Manage Closely sorted by power, or every silent blocker with their org unit) — showing the map is genuinely queryable, not just a table you filled in once.

---

## Step 3 — Build the communication plan

Using the `comms_plan` table, produce one row per stakeholder with a real audience-specific message focus, channel, and cadence (Lecture 2, §1). In `communication-plan.md`:

5. For **at least 3 stakeholders spanning at least 3 different quadrants**, write out the full engagement rationale in prose (not just the table row) — why this channel, why this cadence, and one concrete example of what a real message to them would say this week (a sentence or two, in the style of Lecture 2, §2's "one message, many audiences" examples).
6. Insert all rows into `comms_plan` and paste a query showing "what's due this week," sorted by `next_due`.

---

## Step 4 — Write a sample status report

Choose a specific moment in your project (if using Atlas, you may reuse the sprint-8 scenario from Exercise 2/Challenge 2, or invent a different moment for your own project). In `status-report.md`:

7. Write a complete status report following Lecture 2, §4's one-glance format, with all four RAG values justified against Lecture 2 §3's observable criteria.
8. Insert it as a row in `status_log`.
9. Write **two audience-shaped versions** of the same underlying status — one for a Manage Closely stakeholder, one for a Keep Satisfied or Keep Informed stakeholder — demonstrating Lecture 2, §2's principle: same facts, different framing and depth.

---

## Step 5 — One escalation or change request, tied to your status

In `escalation-or-change.md`, produce **one** of the following, grounded in the status you wrote in Step 4:

- A full four-part escalation (Lecture 3, §1–2) for whatever the top risk in your status report is, addressed to the correct decision-maker per your stakeholder map's Manage Closely quadrant.
- A change-request log entry (Lecture 3, §4) for a plausible scope change someone in your cast might request, evaluated against the charter's objective, the triple-constraint cost, and the correct decision authority.

---

## Deliverable

A directory in your portfolio `c39-week-07/mini-project/` containing:

1. `stakeholder-map.md` — scored, justified, quadrant-placed cast + query results (Step 2).
2. `communication-plan.md` — per-audience rationale + query results (Step 3).
3. `status-report.md` — full status + two audience-shaped versions (Step 4).
4. `escalation-or-change.md` — one real escalation or change request (Step 5).
5. Your SQL (`setup.sql` or inline in the markdown files) showing the actual `INSERT`/`UPDATE` statements you ran.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Stakeholder scoring & quadrants | 20% | Every score justified with a specific reason; quadrant placements defensible under Lecture 1's definitions |
| Silent blocker analysis | 10% | Concrete, specific failure scenarios — not generic warnings |
| Communication plan | 20% | Channel/cadence genuinely fits each audience; rationale ties back to the stakeholder's actual power/interest profile |
| Status report honesty | 25% | RAG values match the observable criteria exactly; no buried slip; one-glance block is genuinely scannable in seconds |
| Audience-shaped versions | 10% | Same facts, real difference in framing/depth, nothing contradicts between versions |
| Escalation/change request | 15% | Follows the four-part template or the three-question evaluation exactly; ask is specific and actionable |

---

## Stretch goals

- Extend `status_log` with a **second** status report from an earlier or later week, and write a short `trend.md` comparing the two — did schedule RAG move, and does the trend line up with the events you'd expect from Weeks 6–7's story so far?
- Build the two audience-shaped status versions (Step 4, Task 9) for a **third** audience in the Monitor quadrant, and reflect in one paragraph on how little that version needs to say compared to the other two.
- If you used Atlas, write the stakeholder map, comms plan, and one status report for the **second** initiative from Exercise 1 (Onboarding Copilot) as a fully separate `org_unit`, and compare in a short paragraph how differently you'd run communications for a small internal tool versus a customer-facing feature with revenue on the line.

---

## Why this matters

Every artifact from Weeks 1–6 — the charter, the backlog, the sprint plan, the risk register, the critical path — is only as useful as the trust the people around it have in what you tell them about its status. A PM who plans perfectly but communicates badly loses stakeholders' confidence anyway; a PM who communicates honestly and clearly can survive real slips, real conflicts, and real bad news with the relationship intact. This mini-project is the shape of that half of the job, held to the same rigor as the planning half. Keep these files — Week 8's metrics dashboard and Week 10's release plan will both assume you can report status honestly on real numbers, not just describe the practice in the abstract.

When done: push, then take the [quiz](../quiz.md) and start Week 8 — Delivery Metrics & Flow.
