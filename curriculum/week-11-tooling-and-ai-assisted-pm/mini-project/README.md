# Mini-Project — Automate a Board and Build an AI-Assisted PM Workflow

> Ship the thing Marcus actually needs by Friday: a GitHub Projects board that maintains itself, and an AI-assisted workflow that drafts a backlog and generates a status + risk digest from real exported data — grounded, verified, and ready to hand to Priya Chen without a second thought.

**Estimated time:** 3.5 hours, best spread across Friday and Saturday after the exercises and challenges.

This is the week's capstone, and it's deliberately the same shape as a real PM's Friday: automate the busywork, use AI where it saves real time, and never forward a number you didn't check. You've built every piece already — Exercise 1's automation, Exercise 2's query fluency, Exercise 3's backlog-grading discipline, Challenge 1's honest schema-reconciliation habit, and Challenge 2's grounded-prompt pipeline. This mini-project assembles them into one deliverable.

---

## Deliverable

A directory in your portfolio `c39-week-11/mini-project/` containing:

1. `board-setup.md` — your GitHub Projects board configuration (fields, views, automations) — reused from Exercise 1, extended per Part 1 below.
2. `export_and_load.py` — a script that exports the board (or, if you're working entirely from the seed, transforms `board_items`) into an analysis-ready pandas DataFrame and a SQL table.
3. `backlog/` — an AI-drafted-then-refined backlog for the brief below, INVEST-graded like Exercise 3.
4. `status_digest.py` — the grounded status + risk digest generator, extending Challenge 2's pipeline with a risk section (Lecture 3, Section 5).
5. `friday-report.md` — the actual output: the digest as Priya would receive it, plus one paragraph of your own commentary on what it got right and what still needs a human's judgment.

Everything runs against `board_items` (extend it with your own rows if you completed Exercise 1's live board) and, optionally, `jira_issues` if you also want the digest to note the Platform-team dependency blocking issue #44.

---

## Part 1 — Automate the board (45 min)

Take the board you configured in Exercise 1 (or, if you skipped the live-tool portion, design this against the `board_items` seed as if it were live) and add **one automation not covered in the exercise**: research and configure (or script, if the built-in workflows don't cover it) a rule that flags anything `Blocked` for more than 5 days — either a label auto-applied via a small script against the GraphQL API, or, if you want to push further, a scheduled GitHub Action that runs daily and comments on stale-blocked items. Document exactly what you built and why in `board-setup.md`.

## Part 2 — Export cleanly (30 min)

Write `export_and_load.py`: pull the board's current state (real API if you have a live board; a direct read of the `board_items` seed table otherwise) into a pandas DataFrame, and confirm it round-trips to SQL with `df.to_sql(...)` and back with `pd.read_sql(...)` producing identical row counts. This is the same discipline as Week 8's export pipeline, aimed at this week's tool.

## Part 3 — Draft the backlog (45 min)

Using the SLA dashboard feature brief from Lecture 3 / Exercise 3, run the grounded backlog-drafting prompt, then apply the same INVEST grading and refinement pass from Exercise 3. This time, add one more step: **for each refined story, add it as a real item to your GitHub Projects board** (or, if working from the seed, write the `INSERT` statements that would add them to `board_items` with a new `iteration = 'Sprint 9'`), with `Status = Todo`, a `Priority`, and a `Size` you estimate yourself — closing the loop from "AI draft" to "actual backlog item a team could pull from."

## Part 4 — Build the grounded status + risk digest (60 min)

Extend Challenge 2's `status_digest.py` into `status_digest.py` for this project with **two sections**, each following the full three-stage grounded pattern independently:

1. **Status** — same as Challenge 2: counts by status for the current iteration, every Blocked item named individually.
2. **Risk** — using Lecture 3, Section 5's "stalled item" query (no status change in N days) *and* a second signal of your choosing (e.g., "P0 items still open," or "items with no assignee"), compute both in SQL, serialize both, and have the AI narrate a risk paragraph that names specific items — never a bare "some things look risky."

Run the full script and capture its output.

## Part 5 — Verify before you ship it (30 min)

Apply Lecture 3, Section 6's three-step verification habit to your own digest output, in writing, in `friday-report.md`:

1. Pick one number in the digest. Re-run its source query by hand. Confirm the match (or find and fix the bug if it doesn't).
2. Read every individually-named item in the digest (Blocked items, risky items) and confirm each is real and correctly described.
3. Ask what's missing — is there a `board_items` row that *should* have shown up in the risk section under your own stated definition of risk, but didn't? If you find one, fix the query, not the prose.

---

## Milestones

- **Milestone 1 (1h):** Parts 1–2. Board automation extended, export pipeline verified.
- **Milestone 2 (45 min):** Part 3. Backlog drafted, graded, refined, and added to the board.
- **Milestone 3 (1h):** Part 4. Grounded status + risk digest running end to end.
- **Milestone 4 (30 min):** Part 5. Verification pass written up in `friday-report.md`.

---

## Rules

- **Every number in `friday-report.md`'s digest section must be traceable to a query you can show.** If you can't point to the exact `SELECT` that produced a figure, it doesn't belong in the report.
- **The AI never invents a name, a count, or a date.** Its job in this project is narration of numbers you computed — exactly Lecture 3's rule, applied for real.
- **The backlog items you add must pass your own INVEST grading** — don't ship a story you graded ❌ on Testable without fixing it first.
- **Document your automation's actual behavior**, not its intended behavior — if you built a script and it didn't quite work as designed, say so and say what you'd fix next.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Board automation | 20% | A real, working rule beyond the exercise's two; documented clearly |
| Export pipeline | 15% | Round-trips cleanly; no spreadsheet anywhere in the chain |
| Backlog quality | 20% | INVEST-graded honestly, refined stories are genuinely small/testable/independent |
| Grounded digest | 30% | Every claim traceable; Blocked/risky items named individually, not just counted |
| Verification | 15% | The three-step check is actually performed and documented, not just described |

---

## Reflection (add to `friday-report.md`, ~200 words)

1. Which part of this workflow saved you the most real time — the board automation, or the AI drafting?
2. Where did you catch yourself almost trusting an AI-generated claim you hadn't verified? What made you check?
3. If you ran this same pipeline every Friday for a real team, what would you automate next that this mini-project still does by hand?
4. One sentence connecting this week back to Week 8: how is a "grounded status digest" the same discipline as "compute in SQL, narrate in prose" that you used for flow metrics — just with an AI doing the narrating this time?

---

## Why this matters

This is the actual shape of modern PM work: two industry-standard tools that hold your team's real data, and an AI layer that's either a genuine multiplier or a genuine liability depending entirely on whether you fed it numbers you computed or let it guess. Do this once, deliberately, with the verification habit built in from the start, and it becomes the default way you'd stand up status reporting on any real team — not a one-time school exercise you did carefully because you knew you were being graded.

When done: push, then take the [quiz](../quiz.md) and start [Week 12 — Capstone](../../week-12-capstone/).
