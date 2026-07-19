# Mini-Project — Live Risk Register, Dependency Map & Critical Path

> Build the three connected artifacts a real PM would maintain for one project: a live RAID risk register, a cross-team dependency map, and a computed critical path with the buffering call-outs that follow from it. This is the week's capstone — Lectures 1–3 and every exercise and challenge land in one deliverable.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

A register you build once for a fictional case study is a good rehearsal. A register you build for something *real* — and keep live, connected to an actual schedule — is proof you can do the job under real conditions, where the risks aren't handed to you pre-written. That's the point of this mini-project.

---

## Step 1 — Choose your project

Pick **one**:

- **(A) Your own real project.** Reuse a project from an earlier week's mini-project if you have one (your Week 1 charter, Week 3 backlog, etc.) — building this week's artifacts on top of a project you've already invested in makes the connections between weeks concrete. Otherwise, any real work with at least a few genuine sequenced tasks and at least one dependency on someone or something outside your direct control.
- **(B) Project Atlas**, extended to its final stretch (Sprints 6–8), reusing and building on this week's lecture material, Challenge 1 option (B), and Challenge 2's dependency chain if you did them.

Write one sentence at the top of your deliverable naming your choice.

---

## Step 2 — Build the live risk register

In SQL (PostgreSQL primary, SQLite fallback — reuse this week's `risks` schema):

1. **Identify at least 8 real risks** for your chosen project, each a full risk statement (cause, uncertain event, consequence).
2. **Score every risk** (probability × impact, 1–5 each) with a one-sentence justification.
3. **Assign a real owner** to every risk.
4. **Assign a response type and concrete plan** to every risk — avoid, mitigate, transfer, or accept — with at least one of each type represented if you can genuinely justify it (don't force an `avoid` or `transfer` that isn't real for your project; explain in a comment if one type is missing).
5. Write **two queries**: a full ranked triage view, and a "needs attention now" filter (high score, open status, thin or missing response plan).

Save this as `risk-register.sql`.

---

## Step 3 — Build the dependency map

In SQL (reuse this week's `dependencies` schema, extended if needed):

1. **Map at least 5 real dependencies** — a mix of internal (within your own team, if applicable) and cross-team/cross-party (another team, a vendor, an external reviewer, a collaborator).
2. For at least **one** dependency, trace it as a **chain** of 2+ hops (A waits on B, who waits on C) — if your real project genuinely has no chain longer than one hop, note that explicitly rather than inventing one; a short, real dependency map beats a padded, fictional one.
3. For every dependency, note **status** and a **coordination owner** — who is actively chasing it, not just who it's assigned to on paper.
4. Apply **at least one** of Lecture 2 §4's coupling-reduction moves (resequence, fallback, own the interface) to a real dependency in your map, and write 2–3 sentences explaining the move and why it reduces risk rather than just tracking it.

Save this as `dependency-map.sql`.

---

## Step 4 — Compute the critical path

1. Lay out your project's remaining (or a meaningful chunk of its) tasks as a table: task, description, duration, predecessors — at least 6 tasks, real ones from your actual project, not filler.
2. Compute the forward pass, backward pass, and slack **by hand** first (`critical-path-by-hand.md`), the way Lecture 3 §3–5 and Exercise 3 did.
3. Confirm it in Python, adapting the Lecture 3 §6 script (`critical_path.py`).
4. State your critical path and total duration clearly.

---

## Step 5 — Connect all three artifacts

In `summary.md` (300–500 words), tie the three pieces together — this is where the week's real insight lives, not in any single artifact alone:

- **Which of your logged risks sit on the critical path**, and which sit on tasks with real slack? (Cross-reference Steps 2 and 4 explicitly — name the task and the risk.) Per Lecture 3 §7, does this change how urgently you'd treat any risk compared to how you scored it in isolation?
- **Which of your dependencies feed into critical-path tasks**, and which feed into tasks with slack? A blocked dependency feeding a critical-path task deserves more of your attention this week than one feeding a task with two weeks of room.
- **Name your single biggest schedule threat right now** — it might be the highest-scored risk, the least-controlled dependency, or simply the critical-path task with the least buffer. Say which, and why, in one clear paragraph.
- **Where would you put a schedule buffer, specifically**, and where would a buffer be wasted? Reference Lecture 3 §7's guidance directly.

---

## Deliverable

A directory in your portfolio `c39-week-06/mini-project/` containing:

1. `risk-register.sql` — Step 2.
2. `dependency-map.sql` — Step 3.
3. `critical-path-by-hand.md` and `critical_path.py` — Step 4.
4. `summary.md` — Step 5, the connective analysis.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Risk register quality | 20% | 8+ real, specific risks; justified scores; concrete, distinct response plans |
| Dependency map quality | 20% | Real internal + cross-team mix; at least one genuine chain traced; a real coupling-reduction move applied |
| Critical path correctness | 25% | Forward/backward pass and slack computed correctly by hand and confirmed in Python; matching results |
| Cross-artifact synthesis | 25% | Explicitly connects specific risks and dependencies to specific critical-path tasks — not three disconnected documents |
| Buffering recommendation | 10% | Specific, justified by actual slack numbers, not generic "add buffer everywhere" |

---

## Stretch goals

- Re-run your critical path assuming your single highest-scored risk actually occurs and adds its worst-case delay to the task it threatens. Does the critical path change? By how much? Does a task that had slack suddenly not have any?
- If you have access to a real team's actual current risks or blockers (work, open source, a class project), interview one person about a risk or dependency they're personally tracking informally, and add it to your register — note in `summary.md` how it changed your ranking.
- Turn your two "needs attention now" and "critical-path risk" queries from Steps 2 and 5 into a single combined SQL view (`CREATE VIEW`) that a whole team could query in one shot each Monday morning.

---

## Why this matters

Every week from here forward assumes you can see trouble coming instead of reacting to it once it's arrived. Week 7's stakeholder and communication management sits directly on top of this week's register and map — you can't run an honest status update (Week 1, Lecture 1 §3c) without a current risk register to report from, and you can't manage stakeholder expectations about a date without knowing the critical path behind it. Keep these three artifacts live — the discipline of updating them as reality changes is the actual job, not a one-time exercise.

When done: push, then take the [quiz](../quiz.md) and continue to Week 7 — Stakeholder & Communication Management.
