# Mini-Project — From Feature Brief to a Sliced, Prioritized, Ready Backlog

> Turn one raw feature brief into a real, working backlog — epics, thin vertical stories, acceptance criteria, and a defended priority order — built and tracked in **GitHub Projects or Jira**, not a Markdown table pretending to be a board.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

This is the week's capstone, and it's the closest thing to the actual day-to-day job of a PM or product owner: someone hands you a messy paragraph of what customers want, and your job is to turn it into work a team can actually plan a sprint against — without losing the intent behind the ask along the way.

---

## The brief

Elena sends you this after a fourth round of customer interviews, this time focused on how new teams get started with Team Workspaces:

> Subject: onboarding is rough — new customers are confused
>
> Talked to two brand-new accounts this week (signed in the last month) and both said basically the same thing: they create their first workspace, and then... they're not sure what to do. One admin said "I made the workspace, invited my team, and then everyone just sat there because there was nothing in it yet." Another said they wished there was some kind of starting point instead of a totally blank workspace.
>
> Specific things people said:
> - "A blank workspace felt like a dead end — give me a starting point"
> - One customer wanted to be able to save a workspace setup (which dashboards are pinned, what the default filters are) as a **template** so their *next* team workspace doesn't start from zero either
> - Someone asked if there's a **guided first-run checklist** ("invite your team," "pin your first dashboard," "leave a comment") — said competitor products do this and it "made it obvious what to do next"
> - One admin specifically complained that when they invited their team, nobody got a clear signal for **what workspace they'd just joined or what to do in it** — they just showed up with no context
> - A newer, smaller customer said they didn't even find the "create workspace" option for two days after signing up — it wasn't clear where to start at all
>
> No hard deadline, but Priya's framing this as directly tied to the same enterprise-retention conversation as everything else — new accounts that churn in month one never get the chance to become month-twelve renewals. Can you turn this into a plannable backlog?

---

## Deliverable

A directory in your portfolio `c39-week-03/mini-project/` containing:

1. `elicitation.md` — at least 5 open questions the brief leaves unanswered, each with a stated assumption you're making to proceed (same discipline as Challenge 1).
2. `backlog.md` — the full written backlog: one epic-level summary, then every sliced story in role–goal–benefit form, each tagged with the slicing pattern used and a brief INVEST pass/fail.
3. `acceptance-criteria.md` — Given/When/Then scenarios (happy path + at least 2 edge cases) for your top 5 highest-priority stories.
4. `prioritization.md` — a full MoSCoW pass over every story, **plus** a WSJF table for just the "Must have" bucket (so ties within your highest-priority tier are broken defensibly, not by gut feel), and a one-paragraph explanation of your final top-5 order in plain language.
5. **A live board** in GitHub Projects or Jira containing every story as a real card/issue, organized by your priority order (a "Ready" column/status populated with everything that's passed Definition of Ready), with a link or screenshot saved in `board-link.md`.

---

## Milestones

Pace yourself; don't try to do all of this in one sitting.

- **Milestone 1 (30 min):** Elicitation — read the brief twice, list your open questions and assumptions before writing a single story.
- **Milestone 2 (60 min):** Epic + full sliced backlog (aim for 8–12 stories), INVEST-checked.
- **Milestone 3 (45 min):** Acceptance criteria for your top 5.
- **Milestone 4 (30 min):** MoSCoW + WSJF prioritization, written justification.
- **Milestone 5 (45 min):** Stand up the real board (GitHub Projects or Jira), create every story as a card/issue, order it, mark the top tier Ready.

---

## Rules

- **No horizontal-layer stories.** If a story is named after a technical layer (schema/API/UI), it fails this mini-project's core requirement — go back to Lecture 2 §3.
- **Every story needs a real benefit**, not a hollow one (Lecture 1 §3). If you can't write a genuine "so that," that's a signal to elicit more before writing the card.
- **The board must be real and live**, not a Markdown table styled to look like one — a link or screenshot in `board-link.md` is required. Free tiers of both GitHub Projects and Jira are sufficient; see [resources.md](../resources.md) for setup links.
- **At least one story must directly address the "can't find create workspace" onboarding-discovery complaint** and at least one must address the **template** idea — don't let the loudest or easiest-to-build items crowd out ones that are quieter but clearly tied to real customer pain in the brief.
- **State your MoSCoW and WSJF reasoning in writing** — a priority order with no visible justification does not satisfy this mini-project, no matter how sensible it looks.

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Elicitation quality | 15% | Real, specific gaps identified; assumptions clearly flagged, not silently baked in |
| Slicing & INVEST | 25% | Every story is thin, vertical, independently valuable, and visibly INVEST-checked |
| Acceptance criteria | 20% | Given/When/Then is precise, observable, and includes genuine edge cases |
| Prioritization rigor | 20% | MoSCoW buckets and WSJF scores both justified in writing; the two methods agree or the disagreement is explained |
| Working board | 20% | A real, live GitHub Projects or Jira board exists with every story tracked and the top tier marked Ready |

---

## Stretch goals

- **Story-map the whole backlog** (Lecture 2, Exercise 2's stretch goal) — lay every story out along the new-customer journey (discover → create → onboard → templatize) and draw the release-1 cut line visibly on the board (a swimlane, a milestone, or a label).
- **Write the Definition of Ready and Definition of Done** you'd actually hold this backlog to (Lecture 3 §3), as a pinned doc/README on the board itself, not just in your portfolio.
- **Simulate Challenge 2**: invent one plausible outside event (a specific competitor move, a specific lost deal) that would force you to reprioritize this exact backlog, and write the one-paragraph message you'd send explaining the reshuffle.

---

## Why this matters

Every skill this week compounds into this one exercise: eliciting real intent from a messy brief, slicing thin enough to get real feedback fast, writing acceptance criteria precise enough that "done" isn't a debate, and prioritizing with a method you can actually defend to your VP. Do this with a real board, not a document, because a backlog that only exists in a Markdown file isn't a backlog — it's a plan for a backlog. Keep this board; Week 4 (Estimation & Sprint Planning) pulls stories directly off it to build a real two-sprint plan against team capacity.

When done: push your deliverables, take the [quiz](../quiz.md), and start Week 4 — Estimation & Sprint Planning.
