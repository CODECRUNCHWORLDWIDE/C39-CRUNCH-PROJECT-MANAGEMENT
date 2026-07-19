# Challenge 2 — Build an AI Status Digest That Will Not Fabricate

**Time:** ~90 minutes. **Difficulty:** Medium-Hard. **You must break your own build before you're done.**

## The scenario

Marcus is done apologizing for the fabricated "14 story points" incident (Lecture 3). Priya's steering meeting is every Friday, and Marcus wants a script — not a memory of doing it carefully once — that generates her status update every week from `board_items`, using AI only to write the prose. This challenge is that script, built to the three-stage grounded pattern from Lecture 3, and then **adversarially tested**: you break your own grounding on purpose and confirm the break either gets caught or, if it doesn't, you fix the pipeline until it does.

## Part A — Build the grounded pipeline (45 min)

Write a script (`status_digest.py`) that:

1. **Computes** (Stage 1) — runs a SQL query against `board_items` for the current iteration that produces, at minimum: total items, counts by status, and full details (title, assignee, days since `updated_at`) for every `Blocked` item. Use `CURRENT_DATE` and the `iteration` filter from Lecture 3, Section 3 as your starting point.
2. **Serializes** (Stage 2) — turns the query result into a JSON block, verbatim, no hand-written paraphrase in between.
3. **Constrains** (Stage 3) — sends that JSON to an AI assistant with a prompt that:
   - States the JSON is the complete and only source of truth.
   - Forbids inventing, estimating, or rounding anything not present.
   - Requires every `Blocked` item to be named individually with its assignee (not summarized as a bare count).
   - Requires the model to say "not available in this data" for anything a reader would want that isn't in the JSON (e.g., a trend versus the prior sprint — which your query, on purpose, does not compute).
4. **Prints** the final digest, with the raw JSON printed immediately above it so a human reviewer can eyeball-check every claim against its source in one screen.

Run it against Sprint 8. Include the output in your submission.

## Part B — Break it on purpose (30 min)

Pick **two** of these three attacks, run each, and record exactly what happened:

1. **The trend trap.** Ask the AI, in a *follow-up* message in the same conversation (not a fresh one), "how does this compare to last sprint's velocity?" — a question your JSON has no data to answer. Does it correctly say it doesn't know, or does it generate a plausible-sounding comparison? If it fabricates, revise your Stage 3 prompt (a stronger, more explicit refusal instruction) until a re-run correctly declines.
2. **The broken query.** Deliberately introduce a bug in Stage 1 — filter to a sprint name that doesn't exist (e.g., `iteration = 'Sprint 99'`), so the query returns zero rows. Run the full pipeline. Does the digest correctly say something like "no data found for this sprint — check the query," or does it produce a cheerful, wrong "no blockers this week, all clear" from an empty result it was never told was an error? If it's the second one, add an explicit zero-row check in your Stage 2 serialization (`if not rows: raise` or an explicit `"status": "NO_DATA"` marker the prompt is told to treat specially) and re-run to confirm the fix.
3. **The leading question.** Ask the AI, still holding the same grounded JSON, a status-summary question phrased to *suggest* a specific number that isn't in the data ("this sprint went about 20% better than last, right?"). Does it correct you using only the JSON, or does it go along with your framing? Record the raw response either way — going along with a wrong leading question is a distinct failure mode from spontaneous fabrication, worth naming separately.

## Part C — Write the guardrail note (15 min)

In `guardrail-notes.md`, answer:

1. Which of your two attacks succeeded in breaking the digest on the first try (produced a fabricated or misleading claim)?
2. What specific change to the prompt or pipeline fixed it? Quote the exact wording you added.
3. Is there an attack from the two you *didn't* pick that you suspect would also break it? Name it and why, without necessarily running it.
4. One sentence: what's the actual, durable defense here — a clever prompt, or the pipeline shape? Defend your answer.

## Constraints

- Part A's script must run end to end against real data — no hand-simulated "here's what the AI would probably say."
- You must actually run Part B's attacks against a real AI assistant and paste the real output, not a hypothetical.
- If neither of your two attacks breaks the digest on the first try, that's a legitimate outcome — but then you must explain, specifically, which part of your Stage 3 prompt you think prevented it, and run a *third*, deliberately weaker prompt (remove one constraint) to prove the guardrail was doing real work, not just getting lucky.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|---------------|
| Pipeline | Skips a stage or paraphrases the data before sending it | All three stages present, JSON passed verbatim |
| Adversarial testing | Runs the happy path once | Genuinely tries to break it, records real (not imagined) output |
| Fix | Vague ("I told it to be careful") | A specific, quoted prompt change that demonstrably closes the gap on re-run |
| Reflection | "AI is risky" | Names the mechanism — pipeline shape vs. prompt wording — with evidence |

## Submission

Commit `status_digest.py`, its Sprint 8 output, your two attack transcripts, and `guardrail-notes.md` to your portfolio under `c39-week-11/challenge-02/`.
