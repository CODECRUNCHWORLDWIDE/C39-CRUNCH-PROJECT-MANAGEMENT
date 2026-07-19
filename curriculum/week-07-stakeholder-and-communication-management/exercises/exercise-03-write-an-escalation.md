# Exercise 3 — Write an Escalation

**Goal:** Turn a real blocker into a four-part escalation (fact, impact, ask, recommendation) that a busy sponsor could act on in one sentence. By the end, you can tell the difference between a complaint and an escalation on sight — yours and other people's.

**Estimated time:** 1 hour.

## Setup

Have Lecture 3, §1–2 open — the four-part template and the weak/strong examples.

## Part A — Diagnose three real escalations

Below are three messages a junior PM actually sent. For each, in `diagnosis.md`:

1. Identify which of the four parts (fact, impact, ask, recommendation) are present and which are missing.
2. Rewrite it as a strong, four-part escalation. Invent reasonable specific numbers/dates where the original is vague — a real escalation always has real numbers.

**Message 1:** *"Design Systems still hasn't given us the updated button component and it's honestly getting frustrating, we've been waiting for like two weeks."*

**Message 2:** *"Just so you know, QA found a bunch of bugs in the comments feature. Might need more time."*

**Message 3:** *"Sofia's team said the API fix will be ready 'sometime next week' — not sure what that means for us exactly but wanted to flag it."*

## Part B — Write a real escalation from scratch

Using the scenario from **Exercise 2** (the sharing-API sandbox outage, 1 day of buffer left, comments integration testing needs 3 days once unblocked, GA was targeted for the Monday after sprint 8), write a complete, four-part escalation to Priya Chen in `escalation.md`.

Your escalation must include:

3. **The fact** — the sandbox outage status and the buffer math, with real numbers from the Exercise 2 scenario.
4. **The impact** — tied explicitly to the GA date and, if you can, to the charter's underlying objective (Week 1 — the at-risk enterprise account renewals GA was timed around).
5. **The ask** — at least two concrete options Priya could choose between (e.g., hold the date and accept a compressed/parallel testing approach if the API comes back Friday vs. Priya formally moving the GA date vs. cutting something from this release to protect the date). Each option should have a one-clause consequence attached.
6. **Your recommendation** — state which option you'd pick and why, in one or two sentences, using the triple-constraint framing from Week 1, Lecture 3, §3.

## Part C — Apply the gut-check

7. In `escalation.md`, after your escalation, answer in 2–3 sentences: could Priya respond to your escalation in one sentence? If not, what's still missing? Revise until the honest answer is yes.

## Expected result (spot checks)

- Message 3's rewrite should replace "sometime next week" with a specific date range and should explicitly ask what happens if it slips past that range — vague timeframes from a dependency owner are exactly the kind of fact a real escalation should force into specifics before it goes to a sponsor.
- Your Part B escalation should **not** ask Priya to simply "make the API faster" — that's not a decision she can make. Every ask must be something the reader actually has the authority and ability to decide.
- Your recommendation in Task 6 should pick one option and defend it, not present all options neutrally and say "up to you" — Lecture 3, §2 is explicit that a recommendation is part of a *strong* escalation, not overstepping.

## Done when…

- [ ] All three Part A messages are correctly diagnosed (missing parts identified) and rewritten in full four-part form.
- [ ] Your Part B escalation has real numbers pulled from the Exercise 2 scenario, not invented ones that contradict it.
- [ ] At least two genuinely different options are offered in the ask, each with a stated consequence.
- [ ] A clear recommendation is stated, with reasoning, not left neutral.
- [ ] Task 7's gut-check honestly identifies whether the escalation is a true one-sentence-response document.

## Stretch

- Write the one-sentence reply you imagine Priya actually sending back to your escalation. If you can't easily imagine a clean one-sentence reply, that's a signal your escalation isn't as tight as you think — go back and fix it.
- Take Message 1 (Design Systems) and write it as an escalation to **Derek Huang directly** instead of to a sponsor — note how the ask changes when you're escalating sideways to a peer team lead instead of upward to someone with authority over your project specifically. What can you legitimately ask for from a peer that's different from what you'd ask a sponsor?

## Submission

Commit `diagnosis.md` and `escalation.md` to your portfolio under `c39-week-07/exercise-03/`.
