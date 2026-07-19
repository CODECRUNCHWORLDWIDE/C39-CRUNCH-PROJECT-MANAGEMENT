# Exercise 3 — Use AI to Draft and Refine a Backlog

**Goal:** Use AI to turn a feature brief into a candidate backlog, then do the PM's actual job — grade it against INVEST (Week 3) and fix what's wrong.

**Estimated time:** 60 minutes.

## Setup

- Have an AI assistant open (Claude, ChatGPT, or similar).
- Re-read Week 3's INVEST criteria if it's fuzzy: [`../../week-03-backlog-user-stories-and-requirements/`](../../week-03-backlog-user-stories-and-requirements/).
- Create a file `backlog-draft.md`.

## The brief

This is the same brief from Lecture 3, Section 4 — use it verbatim so your output is comparable to a classmate's:

> Northlight wants Atlas's SLA dashboard (currently blocked on the Platform team's webhook feed contract — see PLAT-505) to show a public-facing uptime page once the feed is live: rolling 30-day uptime %, incident history with root-cause summaries, and a subscribe-for-alerts email capture. No auth required to view; incident detail requires an internal login.

## Part A — Draft (15 min)

1. Send the AI the exact prompt from Lecture 3, Section 4 (reproduced below), with the brief above pasted in:

   ```
   Given this feature brief, draft 5-8 candidate user stories in the format
   "As a <role>, I want <capability>, so that <benefit>." For each, list
   1-3 acceptance criteria as Given/When/Then. Flag any story you think is
   too large to fit in a single 2-week sprint, and say why.

   Feature brief:
   <paste the brief>
   ```

2. Paste the raw output into `backlog-draft.md` under a heading `## Raw AI draft`, unedited.

## Part B — Grade it against INVEST (25 min)

3. For **every** story the AI produced, score it against all six INVEST letters (Independent, Negotiable, Valuable, Estimable, Small, Testable) — a quick ✅/⚠️/❌ per letter is enough, with one clause of reasoning for any ⚠️ or ❌.

4. You should find, somewhere in the draft, at least one of these three common AI-backlog failure patterns (if you don't on the first pass, look harder — one of them is there):
   - **Two stories that are really one** (e.g., "view uptime %" and "view the uptime page" split for no real reason).
   - **One story that's secretly three** (e.g., a single story bundling the public uptime page, the incident history, *and* the email capture — three independently shippable, independently valuable pieces jammed together).
   - **An acceptance criterion that reads well but isn't testable** (e.g., "the page loads quickly" — quickly by what measure?).

## Part C — Fix it (20 min)

5. Rewrite the backlog by hand: merge what should be merged, split what should be split, and rewrite every untestable acceptance criterion into a Given/When/Then a QA engineer could actually execute against. Put this under `## Refined backlog` in the same file.

6. Write a **3–5 sentence** note under `## What I changed and why` — specifically what INVEST violation each fix addressed. "I made it better" is not a note; "Story 4 bundled three independently valuable capabilities (uptime %, incident history, email capture), which fails Independent and Small — split into three stories, each shippable on its own" is.

## Done when…

- [ ] `backlog-draft.md` has all three sections: raw draft, INVEST grades, refined backlog + notes.
- [ ] Every story in the raw draft has all six INVEST letters graded.
- [ ] You've named which specific failure pattern(s) from step 4 showed up in your draft.
- [ ] Your refined backlog's acceptance criteria are all genuinely testable — read each one and ask "could a QA engineer with no other context execute this?"

## Stretch

- Re-run Part A with a **tighter** prompt that explicitly states the INVEST criteria and asks the AI to self-check against them before returning the draft. Compare: did explicitly naming INVEST in the prompt produce a meaningfully better first draft, or about the same amount of editing work? Write two sentences on what you found — this is a small, personal experiment in **how much prompting effort is worth it** versus just doing the edit pass yourself either way.

## Submission

Commit `backlog-draft.md` to your portfolio under `c39-week-11/exercise-03/`.
