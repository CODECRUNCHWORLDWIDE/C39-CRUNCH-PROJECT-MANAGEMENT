# Challenge 1 — Align Conflicting Sponsors

**Time:** ~90 minutes. **Difficulty:** Medium-hard. **No single right answer.**

## The scenario

Atlas is in sprint 8, one week from GA. Two stakeholders with real, overlapping power have just told you, separately and within an hour of each other, two incompatible things.

**Priya Chen (VP Product, sponsor)**, in a Slack DM: *"The sharing-API situation worries me. I'd rather we hold the GA date firm — enterprise renewal conversations are already scheduled around it — and if comments integration testing can't finish in time, we ship without threaded comments and fast-follow it two weeks later. Comments were always the highest-risk piece; better to ship the reliable 90% on time than the full 100% late."*

**Amara Osei (VP Sales)**, in a separate message twenty minutes later: *"I need Atlas to ship complete, comments included, even if it's a few days late. I've already told two of the three at-risk accounts that 'full collaboration, including discussion threads' is coming at GA — if we ship without comments, I have to go back to them with a downgraded pitch, and one of them (the shakiest of the three) has been looking for a reason to churn. A few days late with the full feature set is a much easier conversation for me than on-time-but-incomplete."*

Both are talking about the **same** underlying decision — the sharing-API buffer squeeze from Lecture 2/Exercise 2 — and they want opposite resolutions. Priya wants to protect the date by cutting comments; Amara wants to protect full scope by moving the date. Both have legitimate, defensible reasoning tied to the same charter objective (retaining the three at-risk accounts, Week 1). Neither has formally overruled the other yet — they've each told *you*, not each other.

## Your task

You are not the decision-maker here — Priya, as sponsor, ultimately has the final call per Week 1's decision-authority section. But you are the person who has to get both of them to a real, informed decision instead of letting this resolve by whoever messages the other first, or by you quietly picking a side and hoping it works out.

Produce a single document, `alignment-plan.md`, with these sections:

1. **Separate positions from underlying needs** (Lecture 3, §5, step 1). State each sponsor's stated position and your best read of their actual underlying need. Are the needs as incompatible as the positions, or is there room neither position alone considered?

2. **Lay out the facts neutrally.** Write the single, shared set of facts (buffer remaining, testing time needed, what's actually known vs. uncertain about the API's return) that both Priya and Amara need to see identically — no framing that favors either side. This is the neutral-facts step from Lecture 3, §5, step 2.

3. **Name the real trade-off, explicitly, in the language you'd actually use to bring both of them into one conversation (or a shared thread).** Write the actual message or meeting-opener you'd send — not a description of what you'd say, the real words.

4. **Propose at least one option neither of them has stated.** Given what you know about Atlas (sprint structure, what shipped already per Exercise 2, the security-review constraint from Week 1), is there a third path — a partial comments feature, a fast-follow with a specific committed date Amara could credibly tell the account, a phased rollout to the three at-risk accounts specifically before general availability? You don't have to invent something perfect — you have to show you tried before defaulting to "pick one of the two stated options."

5. **State what you would do if they still can't agree** after seeing the same facts and the trade-off named explicitly. Per Week 1's decision authority, who actually has the final call, and how do you communicate that respectfully to the person whose preference doesn't win, so they don't feel unheard even though the decision didn't go their way?

## Constraints

- You may not simply agree with whichever sponsor messaged you first, or whichever one has more formal authority, without doing the work in steps 1–4 — a document that skips straight to "Priya's the sponsor, so we do what she says" fails this challenge even if that's ultimately the right outcome, because it skips the part where Amara's legitimate need gets heard and possibly incorporated.
- You may not propose "just ask engineering to work faster" as your third option — that's not a real trade-off, it's a dodge, and Lecture 3 explicitly warns against unpaid-for scope solutions.
- Use real numbers from the Lecture 2/Exercise 2 scenario (1 day of buffer, 3 days of testing needed) — don't invent numbers that make the problem artificially easier to resolve.

## Hints

<details>
<summary>On finding a third option (Task 4)</summary>

Consider: does "ship without comments at GA, then fast-follow" have to mean "no committed date"? Could you get engineering to commit to a specific fast-follow date *now*, tight enough that Amara could credibly tell her shakiest account "collaboration ships at GA, threaded comments follow two weeks later, guaranteed" — turning an open-ended downgrade into a concrete, still-strong pitch? That's not free (Marcus and Priya would need to agree to prioritize it), but it's a real option worth surfacing before assuming the two positions are the only two choices.

</details>

<details>
<summary>On the "underlying need" gap (Task 1)</summary>

Priya's underlying need isn't really "hold the date" in the abstract — it's "don't damage the credibility of dates we commit to externally." Amara's underlying need isn't really "ship comments" in the abstract — it's "don't have to walk back a specific promise to a specific, shaky account." Once you see it this way, a fast-follow with a *guaranteed, communicated* date might satisfy both underlying needs even though it satisfies neither original position perfectly.

</details>

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Underlying needs | Restates the two positions as-is | Digs to the actual need behind each position and finds where they might not be as opposed as they look |
| Neutrality | Facts framed to favor one side | Same facts, presented identically regardless of audience |
| The actual ask | Vague "let's discuss" | A real, specific message that would genuinely move this to a decision |
| Creativity | Picks Priya's or Amara's option verbatim | A genuine third option grounded in real constraints, not wishful thinking |
| Respect for authority | Ignores who actually decides | Names the real decision-maker and how to close the loop respectfully with the other party |

## Submission

Commit `alignment-plan.md` to your portfolio under `c39-week-07/challenge-01/`.
