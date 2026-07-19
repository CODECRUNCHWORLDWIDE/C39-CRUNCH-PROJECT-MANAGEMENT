# Challenge 1 — Forecast Cost-at-Completion for a Slipping Project

**Time:** ~90 minutes. **Difficulty:** Medium-high. **No single right answer.**

## The scenario

It's December 12th. Atlas's core team just wrapped a hard retro after Lecture 2's numbers made the rounds — Priya Chen (VP Product, sponsor) has asked you, directly, for "the real number: what is this actually going to cost, and are we going to hit it." Not a status color. A number, with a defensible method behind it, by end of day.

You already have two forecasts sitting in your notes from Lecture 2 §7 and this week's other lectures, and they don't agree:

- A **naive time-based** read: roughly 3 of 4 planned months are closed, spend looks "close enough" to the annual number, therefore the budget is basically fine.
- A **CPI-based EAC**: `BAC / CPI = $280,000 / 0.880 ≈ $318,000` — about 13.6% over.

Priya has seen budgets before. She will not accept "somewhere between $280K and $318K" as an answer. Your job is to reconcile the two, pick a defensible number, and bring one clear ask.

## Part A — reconcile the two methods (30 min)

1. **Explain, in plain language, why the naive time-based method is too optimistic here.** It isn't wrong because the arithmetic is bad — it's wrong because of what it silently assumes. Name the assumption.
2. **Compute TCPI** (to-complete performance index) as of the November close: `TCPI = (BAC − EV) / (BAC − AC)`. This answers a sharp, specific question: *what cost-efficiency would the remaining work need to achieve, from this point forward, for Atlas to still land exactly on the original $280,000?* Using November's numbers (`BAC = 280000`, `EV = 180833`, `AC = 205400`), compute TCPI and compare it to Atlas's **actual** CPI so far (0.880). Is the required efficiency close to what the team has actually been achieving, or a large jump? What does that gap tell you about whether the original $280,000 is still realistic?

## Part B — a new data point changes the picture (30 min)

Twelve working days into December (of 19 planned), new actuals and delivery data have come in:

| Metric | Value |
|---|---:|
| December-to-date labor actual | $32,000 |
| December-to-date tooling actual | $1,000 |
| December-to-date vendor actual | $2,750 |
| Story points delivered, cumulative (was 155/240 as of Nov 30) | 180 / 240 |
| December's full-month planned budget (labor + tooling + vendor) | $61,500 |

3. **Recompute AC, EV, and CPI as of December 12**, treating "AC" as the full Sep–Nov total ($205,400) plus these new December-to-date actuals. State your new BAC, AC, EV, and CPI.
4. **Recompute EAC using the CPI method with your new numbers.** Has the forecast gotten better, worse, or stayed about the same compared to the November forecast of ~$318,000? By how much?
5. **A second EAC method exists for when you believe past performance won't repeat**: `EAC = AC + (BAC − EV)` — this assumes all *remaining* work will suddenly be done exactly on-plan from here forward, regardless of the CPI trend so far. Compute this version too. Which of your two December EAC numbers is more optimistic, and which assumption is more defensible given what you know about Atlas's trend from Lecture 2 §4?

## Part C — the ask (30 min)

6. **Pick one EAC number to bring to Priya**, and defend the choice in 3–4 sentences. "I averaged the two" is not a defensible method — state which method you trust more for *this specific project's situation*, and why.
7. **State one clear, specific ask** — not a status update. Options include (pick one, or propose your own): request additional contingency/budget approval for the projected overage; propose a scope cut to bring the forecast back under $280,000; propose extending the timeline to reduce cost pressure per sprint; or recommend proceeding as-is with the overage flagged and accepted. Whatever you pick, make the ask **specific and decidable** — Priya should be able to say "yes" or "no" to it in one sentence, the same discipline Week 7 Lecture 3 taught for escalation.
8. **Name one thing you'd want to know before finalizing this recommendation** that isn't in this week's data (e.g., is the December cost dip in Part B a real trend or noise from only 12 days of data; is there flexibility in the vendor contract; would Priya rather cut scope than take a budget increase).

## Constraints

- Show your arithmetic, not just final numbers — a forecast with no visible work is not a defensible forecast.
- You may use SQL, pandas, or a calculator for Part A/B's math — the method matters more than the tool this time.
- Part C's ask must be a single sentence a busy VP could say yes/no to without follow-up questions.

## How success is judged

| Signal | Weak answer | Strong answer |
|---|---|---|
| Method reconciliation | Picks a number without explaining the disagreement | Explains specifically *why* the naive method undercounts (Part A §1) |
| TCPI reasoning | Skips it or computes it without interpreting it | Uses the TCPI-vs-actual-CPI gap as evidence for or against the original budget |
| Arithmetic | Numbers don't reconcile internally (AC, EV, CPI inconsistent with each other) | December figures are computed correctly and consistently with Lecture 2's method |
| Method choice (Part C) | "I'll just go with the average" | Names a specific reason one EAC method fits Atlas's actual trend better than the other |
| The ask | Vague ("we should discuss options") | One sentence, specific, decidable |

## Submission

Commit `challenge-01.md` to your portfolio under `c39-week-09/challenge-01/`.
