# Lecture 1 — Risk Registers & the RAID Log

> **Duration:** ~2 hours. **Outcome:** You can identify risks from a real project, score them consistently, choose a response strategy, and stand up a live risk register in SQL that a whole team can actually use — instead of a document that goes stale the day it's written.

Every project has risks. The difference between a project that handles them well and one that gets ambushed by them isn't whether risks exist — they always do — it's whether anyone **wrote them down, scored them, assigned an owner, and kept the list alive**. A risk register that lives in someone's head, or in a slide from the kickoff deck that nobody has opened since, is not a risk register. It's a memory of one.

## 1. Where we left Project Atlas

Recall from Week 1: Project Atlas is Northlight Software's initiative to add Team Workspaces (shared dashboards, comments, real-time presence) to their product, Northlight Insights. The cast: **Priya Shah** (VP Product, sponsor), **Elena Cruz** (Product Owner), **Marcus Webb** (Tech Lead) and his engineers, and **you** (PM).

Atlas is now roughly five sprints into an eight-sprint plan. Three things are true right now that weren't true at kickoff:

1. The real-time presence feature depends on a third-party API from a vendor called **Pulse.io** (fictional), and its sandbox environment has been intermittently flaky — sometimes returning stale presence data, sometimes timing out for 10–20 seconds. This was mentioned in passing back in Week 1 as "a risk to watch." It was never formally logged, scored, or assigned an owner. It just sat in a Slack thread titled "pulse being weird again lol."
2. Atlas's sharing feature needs a new capability from the **Platform team** — workspace-scoped permissions — that Platform hasn't shipped yet, and Platform has its own roadmap that doesn't obviously prioritize Atlas.
3. Marcus's team lost a day this week because two engineers built overlapping code for the same comment-threading logic — nobody had mapped that their two stories touched the same module.

None of these are surprises in hindsight. They're exactly the kind of thing a live risk register and a dependency map would have surfaced *before* they cost time. That's this week's job.

## 2. What counts as a risk (and what doesn't)

A **risk** is something that **might** happen and, if it does, would hurt the project — scope, schedule, cost, or quality. This is a specific, narrower definition than "anything I'm worried about," and the distinction matters:

- "Pulse.io's sandbox might stay flaky through launch, delaying real-time presence" → **risk**. It's uncertain (might not happen) and specific (a named, describable event with a describable consequence).
- "The team seems kind of tired lately" → not yet a risk as stated. It's an *observation*. To become a risk, it needs to become specific: "if the team stays at this pace for 3 more sprints, velocity may drop and the release date slips" — now it's scoreable and actionable.
- "Pulse.io's API is timing out in the sandbox this week" → this is no longer a risk, it's an **issue** — something that has already happened and needs a response now, not a probability estimate. (More on the Issues side of RAID in §6.)

**A good risk statement has three parts:** a cause, an uncertain event, and a consequence. *"Because Pulse.io's sandbox is unstable (cause), real-time presence sync might fail intermittently at launch (event), which would degrade the collaboration experience for early adopters and could stall the enterprise renewals Atlas exists to save (consequence)."* Compare that to "Pulse.io risk" — technically not wrong, but useless for scoring, useless for deciding a response, and useless six weeks from now when whoever reads it has forgotten the context.

## 3. Scoring risk: probability × impact

You can't act on every risk with equal urgency — that's how a register becomes noise nobody reads. Score each risk on two independent 1–5 scales and multiply them:

| Score | Probability | Impact |
|:-:|---|---|
| 1 | Rare — unlikely to happen this project | Negligible — barely noticeable if it happens |
| 2 | Unlikely — could happen, but no signal yet | Minor — small delay or rework, easily absorbed |
| 3 | Possible — a coin flip, or early signal exists | Moderate — noticeable schedule/scope/quality hit |
| 4 | Likely — signal is strong, expect it | Major — threatens a milestone or a success criterion |
| 5 | Near-certain — actively happening in some form | Severe — threatens the whole project's viability |

**Risk score = probability × impact**, giving a 1–25 scale. This is not fake precision — you're not claiming "13" is meaningfully different from "12." You're using the score to **rank and triage**: a 20 needs a response plan and an owner checking on it weekly; a 4 might just need to be logged and revisited monthly. The multiplication also captures an important asymmetry: a *near-certain, negligible* risk (5×1=5) and a *rare, severe* risk (1×5=5) score the same, and that's correct — both deserve real but different attention, whereas an *unscored gut feeling* treats the near-certain nuisance and the rare catastrophe identically, which is how catastrophic risks get ignored because they "probably won't happen."

Scoring Atlas's three risks from §1:

| Risk | Probability | Impact | Score |
|---|:-:|:-:|:-:|
| Pulse.io sandbox instability delays real-time presence | 4 (strong signal, weeks of flakiness) | 3 (degrades one feature, not the whole release) | **12** |
| Platform team's workspace-permissions API slips past Atlas's need date | 3 (no committed date yet from Platform) | 4 (blocks the sharing feature entirely — Atlas's core value) | **12** |
| Duplicate work from unmapped internal dependency (comment-threading overlap) | 2 (a one-time miss, now visible) | 2 (a day of rework, already absorbed) | **4** |

Notice the third one, even though it already *happened* this week, still gets logged — as a **closed issue** that becomes a **risk** for the rest of the project ("this class of overlap could happen again unless we change how we split stories" — see Lecture 2 on internal dependency mapping for the actual fix).

**A scoring discipline that keeps the register honest:** score risks with the person who owns the domain, not alone at your desk. Marcus scores the Pulse.io technical risk with you; you don't guess at an engineer's confidence level. Elena or Priya weighs in on impact when it touches scope or the business case. A register you score solo is a register that reflects your anxieties, not the project's actual exposure.

## 4. Response strategies: avoid, mitigate, transfer, accept

Every risk gets a **response type** — a decision about what you're going to *do* about it, not just watch it. There are exactly four honest options:

- **Avoid** — change the plan so the risk can't happen at all. If a risk is severe enough and the workaround is cheap enough, remove the exposure entirely. *Example: if Pulse.io's instability were bad enough and there were a same-week alternative, "avoid" would mean switching vendors before launch rather than betting on a fix.* Avoid is the strongest response and also the rarest — it usually means cutting scope or changing approach, which has its own cost.
- **Mitigate** — reduce the probability or the impact (or both), without eliminating the risk. *Example: build a fallback UI state for when presence data is stale or missing, so a Pulse.io hiccup degrades gracefully instead of breaking the page. This doesn't fix Pulse.io — it shrinks the impact if it acts up.* Mitigate is the most common response for technical and vendor risks.
- **Transfer** — shift the consequence (often the cost) to someone else, typically via a contract, insurance, or a vendor SLA. *Example: escalate to Pulse.io's support with a paid-tier SLA that guarantees a fix window, or negotiate a service credit if the sandbox instability persists into production.* Transfer doesn't reduce the chance of the risk happening — it changes who absorbs the pain if it does.
- **Accept** — consciously decide to do nothing extra, because the score is low enough or a response costs more than the risk is worth. *Example: the duplicate comment-threading work — a 4-score, already resolved — gets accepted with a note, not a formal mitigation plan.* Accept is not the same as ignoring; it's a **documented decision** with a stated reason, made by someone with the authority to make it (often you, sometimes Priya for anything touching the business case).

The trap to avoid: defaulting to "mitigate" for everything because it feels active without committing to anything specific. A mitigation plan with no concrete action ("we'll keep an eye on it") is functionally an unstated *accept* — say what it actually is. Lecture 3's critical-path work will also change how you weigh these: a risk sitting on the critical path deserves a stronger response than the identical risk sitting on a task with three weeks of slack.

## 5. RAID: risks are one quarter of the picture

**RAID** stands for **Risks, Assumptions, Issues, Dependencies** — four related but distinct things that belong in one living log because they interact constantly:

- **Risks** — might happen, hasn't yet (this lecture).
- **Assumptions** — things you're treating as true without full confirmation, because you have to plan on *something*. *"We're assuming Pulse.io's production environment is more stable than its sandbox."* An assumption that turns out false becomes a risk (or an issue) instantly — writing assumptions down is how you notice which ones you're silently relying on.
- **Issues** — risks (or assumption failures) that have already happened and now need an active response, not a probability estimate. *"Pulse.io's sandbox has been down for 6 of the last 10 days"* graduated from risk to issue the day it started actually blocking QA.
- **Dependencies** — things you need from someone else to proceed. Covered in full in Lecture 2, but they live in the same log because a blocked dependency is often exactly how a risk materializes.

This week's schema focuses on Risks and Dependencies as separate tables because they need different columns (a risk needs probability/impact; a dependency needs a needed-by team and date) — but treat Assumptions and Issues as **rows you can add to the same `risks` table** using the `category` and `status` columns: an assumption is a risk row with `status = 'open'` and a note in `description` flagging it as an assumption; an issue is a risk row that's transitioned to `status = 'occurred'`.

## 6. Building the live register in SQL

You already created the `risks` table in this week's setup. Insert Atlas's risks from §3:

```sql
INSERT INTO risks
    (risk_id, title, description, category, probability, impact, owner,
     trigger_event, response_type, response_plan, status, date_logged)
VALUES
(1, 'Pulse.io sandbox instability',
    'Pulse.io''s real-time presence API sandbox intermittently returns stale data or times out 10-20s, threatening the real-time presence feature.',
    'vendor', 4, 3, 'Marcus Webb',
    'Sandbox error rate exceeds 5% over any 24h window, measured via Pulse.io status dashboard',
    'mitigate',
    'Build graceful-degradation UI for stale/missing presence data; escalate to Pulse.io support for an SLA; re-test weekly against production credentials once available.',
    'open', '2026-07-13'),
(2, 'Platform team permissions API slip',
    'Atlas sharing feature requires workspace-scoped permissions from Platform team''s auth service, no committed ship date yet.',
    'schedule', 3, 4, 'You (PM)',
    'Platform has not confirmed a date by end of Sprint 5 planning',
    'mitigate',
    'Escalate to Platform''s EM this week for a committed date; scope a temporary Atlas-side permission shim as a fallback if Platform slips past Sprint 7.',
    'open', '2026-07-13'),
(3, 'Duplicate comment-threading work',
    'Two engineers independently built overlapping comment-threading logic; one day of rework absorbed.',
    'people', 2, 2, 'Marcus Webb',
    NULL,
    'accept',
    'Already resolved this sprint; accepted as a one-time cost. See dependency-mapping fix in Lecture 2 to prevent recurrence.',
    'closed', '2026-07-14');
```

Now the register earns its keep. Instead of scrolling a document, you **query** it:

```sql
-- The triage view: every open risk, worst first
SELECT risk_id, title, probability, impact,
       (probability * impact) AS score,
       owner, response_type, status
FROM risks
WHERE status = 'open'
ORDER BY score DESC;
```

```sql
-- Anything scored high with no real response plan yet — your Monday-morning check
SELECT title, probability * impact AS score, owner
FROM risks
WHERE status = 'open'
  AND (probability * impact) >= 12
  AND (response_plan IS NULL OR response_plan = '');
```

```sql
-- What's on Marcus's plate specifically, across every category
SELECT title, category, probability * impact AS score, response_type
FROM risks
WHERE owner = 'Marcus Webb' AND status = 'open'
ORDER BY score DESC;
```

That last query is the entire point of doing this in SQL instead of a spreadsheet: **a live filter by owner, status, or score, that's always current**, because updating a risk's status is one `UPDATE` statement, not a search-and-edit across scattered tabs and colors:

```sql
UPDATE risks
SET status = 'mitigated',
    response_plan = response_plan || ' -- Pulse.io confirmed SLA on 2026-07-20; sandbox stability improved.'
WHERE risk_id = 1;
```

## 7. Why not a spreadsheet? (the honest case)

It's worth being precise about *why* the data-tooling rule bans spreadsheets here, because "just use SQL" can sound like dogma without the reasoning:

- **Concurrent ownership.** Priya, Elena, Marcus, and you all update this register in the same week. A shared spreadsheet either locks (one editor at a time) or silently overwrites conflicting edits. A database table with `UPDATE ... WHERE risk_id = N` handles concurrent writers correctly by design.
- **Consistent scoring.** A spreadsheet lets someone type "hgh" into the impact column, or a formula reference one row off after an insert shifts things. A `probability INTEGER NOT NULL` column with application-level validation (or a `CHECK (probability BETWEEN 1 AND 5)` constraint) makes bad data impossible to enter, not just unlikely.
- **Joins are coming.** Next lecture's dependency table and next week's schedule table both need to relate back to this risk register (a risk *causes* a blocked dependency; a risk *sits on* a critical-path task). That's a `JOIN`, a native SQL operation. In a spreadsheet, that's a brittle `VLOOKUP` that breaks the moment a row moves.
- **The register outlives any one view of it.** A spreadsheet tab is *a* view. A SQL table is *the data* — you can build ten different filtered views (by owner, by score, by category, by sprint) without ever duplicating or risking desyncing the underlying rows.

## 8. Check yourself

- Write a full risk statement (cause, uncertain event, consequence) for something on a project you're actually involved in — not a one-line label.
- Score two risks that both land on a total score of 12 but with different probability/impact splits (e.g., 4 probability × 3 impact, versus 3 probability × 4 impact). Explain why the *same score* can still call for different responses.
- Give one real example each of avoid, mitigate, transfer, and accept — not from this lecture, from your own experience or a project you know.
- Why does an "accept" response still need to be written down, instead of just... not writing anything?
- Write the SQL query that would show you every risk owned by a specific person, sorted worst-first, regardless of status.

If those are automatic, Lecture 2 moves from *what could go wrong* to *who's waiting on whom* — dependency mapping, the second half of what makes Atlas's schedule fragile or resilient.

## Further reading

- **PMI — "Risk Management Overview":** <https://www.pmi.org/learning/library/risk-management-overview-project-managers-7113>
- **Atlassian — "RAID log explained":** <https://www.atlassian.com/work-management/project-management/raid-log>
- **PostgreSQL — `CHECK` constraints:** <https://www.postgresql.org/docs/current/ddl-constraints.html>
