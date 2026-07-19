# Challenge 1 — Build a Live Risk Register for a Real Project

**Time:** ~90 minutes. **Difficulty:** Medium. **No single right answer.**

## The scenario

Every exercise this week used a fictional project (Meridian) so there was a clean, gradeable answer key. Real risk registers are harder, because nobody hands you a pre-written list of 12 risks with the signal already highlighted — you have to **find** the risks yourself, from a real or realistic situation, before you can score or respond to any of them.

## Your task

Pick **one**:

- **(A) A real project you're actually involved in** — work, school, open source, a side project. This is the stronger option: a register for something you genuinely care about is easier to write honestly and more useful to keep.
- **(B) Project Atlas, extended.** Assume Atlas is now in its final two sprints before launch (Sprints 7–8 of 8). Beyond the three risks already logged in Lecture 1, invent **5 additional, realistic risks** specific to a feature launch's final stretch — think about what tends to go wrong right before a release (load at scale, support-team readiness, rollback planning, marketing/comms timing, data migration for existing customers) rather than reusing early-project risks like "requirements are unclear."

Whichever you pick, build a **live register with at least 10 total risks**:

1. **Identify.** For each risk, write a full risk statement (cause, uncertain event, consequence — Lecture 1 §2). If you picked (A), these must be risks you can actually defend as real for your project — don't pad the count with generic, copy-pasted risks that don't specifically apply.
2. **Score.** Probability × impact for every risk, using Lecture 1 §3's scale, with a one-sentence justification for each score (not just the number).
3. **Assign owners.** A real name or role for each risk — for (A), a real person if the project has a team, or a specific role/hat you'd wear if it's solo work; for (B), use Atlas's cast (Priya, Elena, Marcus, you) plus any team you invent for the new risks.
4. **Respond.** Avoid/mitigate/transfer/accept for every risk, each with a concrete plan (Lecture 1 §4) — at least one of each response type must appear somewhere in your register (if you genuinely can't justify one of the four types, explain why in a comment, don't force it).
5. **Build it in SQL.** A working `risks` table (reuse this week's schema, extend it if your project needs an extra column — justify any addition) with all rows inserted, plus **three distinct queries** you'd actually run day-to-day: a full ranked triage view, a filtered "high score with no response plan" check, and one query of your own design that answers a real question about *your specific* register (e.g., "every open vendor risk," "every risk logged in the last 7 days").

## Constraints

- Minimum 10 risks, but don't pad — every row should be one you'd actually defend as real and specific, not filler to hit the count.
- Every risk needs all of: statement, score with justification, owner, response type, and concrete plan. An incomplete row is worse than one fewer row done properly.
- If you pick (A), you may anonymize real names/companies if needed, but the risks themselves must be genuinely real to your situation — this isn't an invitation to reuse Meridian or Atlas under a different name.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Risk identification | Generic, could apply to any project ("scope might change") | Specific to this project's actual technical, vendor, people, or schedule situation |
| Scoring | A number with no justification, or every risk scored similarly | Justified scores with real spread — not everything is a 5×5 or a 1×1 |
| Response plans | "Monitor closely" repeated across multiple risks | Concrete, distinct actions per risk; at least one real `accept` with a stated reason |
| SQL artifact | Static list dumped into a table once | Working schema + queries that would actually be used week over week |

## Submission

Commit your SQL file(s) and any supporting notes to your portfolio under `c39-week-06/challenge-01/`.
