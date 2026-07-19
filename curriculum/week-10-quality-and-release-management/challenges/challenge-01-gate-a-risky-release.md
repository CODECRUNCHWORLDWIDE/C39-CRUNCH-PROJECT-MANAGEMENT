# Challenge 1 — Gate a Risky Release

**Time:** ~90 minutes. **Difficulty:** Medium-hard. **No single right answer.**

## The scenario

It's Monday, April 27, 2026 — two days before Atlas's planned GA. The go/no-go meeting is in three hours: Marcus Diallo, Yuki Tanaka, Omar Farouk, Sofia Reyes, Elena Cruz, and Priya Chen, per the roles in [Lecture 2, §5](../lecture-notes/02-release-management.md). You're the PM running the meeting, and you're expected to walk in with the actual data queried and summarized, not vibes.

Here's what's true right now, straight from the tables you loaded across the lectures:

- **DoD (`dod_criteria`, `release_name = 'atlas-ga-2026-04-29'`):** 3 of 13 required criteria are `not_met` — the load test, QA sign-off, and Platform Team sign-off — all traced to the same root cause: signature verification can't hold up past 2.1x normal peak load.
- **Known defects (`known_defects`, same release):** ATLAS-131 (major, no workaround, the same signature-verification issue), ATLAS-133 (major, has a workaround — manual dashboard refresh), ATLAS-135 (minor, cosmetic only).
- **Release checklist (`release_checklist`, same release):** 6 of 14 items aren't `done`; one is `blocked` — the 48h staging soak test, blocked by the same underlying issue.
- **The business context:** Priya Chen wants GA timed so Jamie Okafor can walk into next week's QBRs with the three at-risk accounts showing a shipped feature. A slip doesn't just move a date — it changes what Jamie can credibly tell three specific, named accounts, one of which (per Week 7) has been "looking for a reason to churn."

## Your task

Produce `go-no-go-decision.md` with these sections:

1. **Query and summarize the real state.** Write and run the three queries needed to answer "what's blocking us" (Lecture 1 §3's DoD query, Lecture 2 §3's defect query, Lecture 2 §2's checklist query), and paste the actual output. Don't paraphrase from memory — the meeting runs on the query results, not on what you remember from the lecture.

2. **Diagnose the root cause, not the three symptoms.** The DoD's three `not_met` lines, ATLAS-131, and the blocked soak test are not three separate problems — explain, in your own words, why they're one problem wearing three checklist entries, and what that implies about whether fixing "just the DoD line" would actually make the release safe.

3. **Walk each of the six `release_signoffs` roles through their actual decision**, and record it (`go`, `no_go`, or `conditional_go` with the condition stated precisely) — you are not free to invent a consensus that skips this; each role reasons from *their own* stated ownership in Lecture 2, §5's table. Yuki Tanaka's reasoning should differ from Sofia Reyes's, which should differ from Priya Chen's — same facts, different lens.

4. **Reach a final, single decision** for the release as a whole, using Lecture 2, §5's three-outcome matrix. If it's `conditional_go`, state the condition precisely enough that Omar Farouk could act on it without asking a follow-up question (Lecture 2's own worked example — "hold rollout at 10% until success rate holds above 99% for 24 hours" — is the bar for precision).

5. **Write the message you'd actually send Jamie Okafor** immediately after the decision, so she can plan what to tell the three at-risk accounts. Real words, not a description of what you'd say.

## Constraints

- You may not treat ATLAS-135 (the cosmetic Safari bug) as relevant to your decision in any way beyond acknowledging it exists — a strong answer explicitly says "this one doesn't matter to the decision" rather than silently ignoring it, but it does not let a trivial defect pad out the argument on either side.
- You may not resolve this by simply declaring "we ship anyway, QA's objection is noted" — QA's `not_met` sign-off has to be genuinely reasoned through and either resolved with real mitigation or honored as a blocker, not overruled by authority alone. Re-read Lecture 1, §5 on why QA's sign-off carries real weight.
- You may not invent a fix that "just happens" to solve ATLAS-131 by go/no-go time without engineering evidence — if your conditional-go relies on the bug being fixed, say what evidence would need to exist and by when, not just assume it.

## Hints

<details>
<summary>On finding the real root cause (Task 2)</summary>

Ask: does the signature-verification path fail because of Atlas's own code, or because of something the Platform team's webhook infrastructure does under load (the same "flaky sandbox" and rate-limit risk that's been in the risk register since Week 6)? If it's the latter, fixing "Atlas's code" alone may not fix the underlying problem — which is exactly why Sofia Reyes's sign-off, not just Marcus Diallo's, is required in the DoD.

</details>

<details>
<summary>On a defensible conditional-go (Task 4)</summary>

Consider: does GA have to mean "100% of customers, signature verification exercised at full load, on April 29"? Or could a narrower version — e.g., GA to a small first cohort at a traffic level the system has actually proven it can handle, with an explicit, monitored gate before widening further — satisfy Priya's date pressure without pretending the load problem is solved? This is Lecture 2, §4's staged-rollout logic applied under real pressure, not in the abstract.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Uses real data | Restates the scenario prose | Actual query output backs every claim |
| Root cause | Treats 3 DoD lines as 3 problems | Identifies and explains the single shared cause |
| Per-role reasoning | Six identical "I agree" signoffs | Each role reasons from their own stated ownership, and disagrees where it's realistic they would |
| Decision precision | "Let's proceed carefully" | A specific go/no_go/conditional_go with a condition someone could act on without a follow-up question |
| Respect for QA | QA's `not_met` overruled by authority alone | QA's objection genuinely resolved or genuinely honored |

## Submission

Commit `go-no-go-decision.md` to your portfolio under `c39-week-10/challenge-01/`.
