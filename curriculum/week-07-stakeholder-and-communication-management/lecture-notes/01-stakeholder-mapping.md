# Lecture 1 — Stakeholder Mapping & Engagement

> **Duration:** ~2 hours. **Outcome:** You can list every real stakeholder on a project, score each one by power and interest, place them on a four-quadrant grid, name the sponsor and any silent blockers, and write a one-line engagement strategy per stakeholder — stored as a queryable table, not a static diagram you draw once and never open again.

Every project has a cast of people who can help it, slow it, or kill it, whether or not you ever formally "manage" them. The mistake most new PMs make isn't failing to talk to stakeholders — it's talking to all of them the *same amount*, the same way. That wastes the CFO's time with sprint-level detail he never asked for, and starves the one VP who could quietly cancel your project of the one conversation that would have kept her on side. Stakeholder mapping is the discipline of deciding, on purpose, who gets how much of your attention and why.

## 1. Who counts as a stakeholder

A stakeholder is **anyone whose support you need, or whose opposition can hurt you** — not just people who report to you or attend your standups. On Project Atlas, that list is wider than the delivery team:

| Name | Role | Org unit |
|------|------|----------|
| Priya Chen | VP Product — **sponsor** | Product |
| Elena Cruz | Product Owner | Product |
| Marcus Diallo | Tech Lead | Engineering |
| Sofia Reyes | Platform Team Lead | Platform Team |
| Derek Huang | Design Systems Lead | Design Systems |
| Amara Osei | VP Sales | Sales |
| Tom Reilly | CFO | Finance |
| Jamie Okafor | Customer Success Lead | Customer Success |
| (you) | Project Manager | Product |

Notice who's on this list who never touches a sprint board: Tom Reilly has never opened Jira, and Amara Osei has said almost nothing about Atlas since the kickoff. Both are still stakeholders, because both can affect whether Atlas ships, gets funded next quarter, or gets the sales support it needs to actually move the renewal-risk accounts it was built for (Week 1's charter, §Objective).

**A quick test:** if this person disappeared tomorrow, would the project be affected — by losing support, losing information, or losing a blocker they were quietly removing? If yes, they're a stakeholder, whether or not they're in your daily standup.

## 2. Two axes: power and interest

Score every stakeholder on two independent 1–5 scales:

- **Power** — how much this person can affect the project's fate. Can they change the budget, the date, the scope, or kill the project outright? Can they block a dependency your team needs (Week 6, Lecture 2)? A 5 can end the project alone; a 1 has essentially no leverage over it.
- **Interest** — how much this person actually cares, right now, about this specific project's outcome, measured by attention, not sentiment. A 5 checks in constantly and would notice immediately if something went wrong; a 1 would barely notice if Atlas vanished from the roadmap.

These are **independent**. High power does not imply high interest, and that gap is exactly where projects get quietly derailed — a stakeholder with real power who *isn't* paying attention can be blindsided later and react badly, precisely because nobody kept them warm along the way.

Scoring the Atlas cast:

| Name | Power | Interest | Why |
|------|:-:|:-:|-----|
| Priya Chen | 5 | 5 | Sponsor — controls budget, date, and scope trade-offs; checks in weekly by design |
| Elena Cruz | 4 | 5 | Owns the backlog and priorities; lives in the details daily |
| Marcus Diallo | 4 | 5 | Owns technical approach and estimates; day-to-day decision-maker |
| Sofia Reyes | 3 | 2 | Controls a dependency Atlas needs (Week 6), but Atlas is a minor item on Platform's own roadmap |
| Derek Huang | 2 | 2 | Controls design-system component availability; moderate stake, moderate attention |
| Amara Osei | 5 | 1 | Can affect sales messaging, timing pressure, and executive air cover — but hasn't engaged yet |
| Tom Reilly | 4 | 2 | Approves budget continuation; watches burn quarterly, not weekly |
| Jamie Okafor | 2 | 5 | No formal authority over Atlas, but represents the three at-risk accounts the whole objective is built around — extremely invested |

Insert this into the `stakeholders` table (power/interest as scored, `quadrant` computed next, `silent_blocker_risk` flagged for anyone with power ≥ 4 and interest ≤ 2):

```sql
INSERT INTO stakeholders
    (stakeholder_id, name, role, org_unit, power, interest, silent_blocker_risk, notes)
VALUES
    (1, 'Priya Chen',    'VP Product (sponsor)',      'Product',           5, 5, FALSE, 'Signs off scope/date/budget trade-offs'),
    (2, 'Elena Cruz',     'Product Owner',              'Product',           4, 5, FALSE, 'Owns backlog priority'),
    (3, 'Marcus Diallo',  'Tech Lead',                  'Engineering',       4, 5, FALSE, 'Owns technical approach/estimates'),
    (4, 'Sofia Reyes',    'Platform Team Lead',         'Platform Team',     3, 2, FALSE, 'Owns the sharing-API dependency (Week 6)'),
    (5, 'Derek Huang',    'Design Systems Lead',        'Design Systems',    2, 2, FALSE, 'Owns shared component availability'),
    (6, 'Amara Osei',     'VP Sales',                   'Sales',             5, 1, TRUE,  'High power over messaging/timing; not yet engaged'),
    (7, 'Tom Reilly',     'CFO',                         'Finance',           4, 2, TRUE,  'Approves continued budget; low visible interest so far'),
    (8, 'Jamie Okafor',   'Customer Success Lead',      'Customer Success',  2, 5, FALSE, 'Voice of the 3 at-risk renewal accounts');
```

## 3. The four quadrants

Plot power on one axis, interest on the other, and every stakeholder lands in one of four quadrants — each with a different job for you:

```
 High  │  KEEP SATISFIED         │  MANAGE CLOSELY
 Power │  (Amara, Tom)           │  (Priya, Elena, Marcus)
       │  Enough info to stay    │  Full partnership: regular
       │  confident and quiet;   │  1:1s, real detail, involve
       │  don't overload them    │  in decisions
       ├──────────────────────────┼──────────────────────────
 Low   │  MONITOR                │  KEEP INFORMED
 Power │  (Derek)                │  (Jamie)
       │  Light-touch updates;   │  Regular, honest updates;
       │  watch for a change     │  no real authority, but a
       │  in either axis         │  loud and legitimate voice
       └──────────────────────────┴──────────────────────────
          Low Interest              High Interest
```

- **Manage Closely (high power, high interest):** your inner circle. They get real detail, involvement in trade-off decisions, and frequent 1:1 or small-group time. This is Priya, Elena, and Marcus for Atlas — you'd never let one of them find out something important from someone else.
- **Keep Satisfied (high power, low interest):** the quadrant most new PMs under-invest in, and it's the most dangerous to neglect. These people *could* stop your project cold, but aren't watching closely — which means if something goes wrong, they hear about it late, from someone else, with no context, and react from a position of surprise rather than partnership. Give them just enough — a concise digest, a heads-up before anything reaches their attention some other way — to stay confident without demanding your time. This is Amara and Tom.
- **Keep Informed (low power, high interest):** they can't change the project's direction, but they have a legitimate stake and a voice worth respecting — and ignoring them is both unkind and risky, since a neglected high-interest stakeholder often finds an indirect way to apply pressure (going over your head, escalating through someone with power). Regular, honest, plain-language updates. This is Jamie — Customer Success can't set Atlas's scope, but they're fielding real customer questions and deserve to know what's actually true.
- **Monitor (low power, low interest):** light touch. Include them in broad updates (a release notes channel, a public status page) without spending 1:1 time. Watch for movement — if either axis changes, re-map them. This is Derek, for now — though if Design Systems becomes a bigger blocker later, he moves quadrants and your investment should move with him.

Update the table:

```sql
UPDATE stakeholders SET quadrant = 'manage_closely' WHERE stakeholder_id IN (1,2,3);
UPDATE stakeholders SET quadrant = 'keep_satisfied' WHERE stakeholder_id IN (6,7);
UPDATE stakeholders SET quadrant = 'keep_informed'  WHERE stakeholder_id IN (8);
UPDATE stakeholders SET quadrant = 'monitor'        WHERE stakeholder_id IN (4,5);
```

Query it back sorted by how much attention each quadrant demands:

```sql
SELECT name, role, power, interest, quadrant, silent_blocker_risk
FROM stakeholders
ORDER BY power DESC, interest DESC;
```

## 4. Finding the sponsor

Every project needs exactly one person who can answer, definitively, "is this still worth doing" and "what wins if scope, time, and cost can't all hold" (Week 1, Lecture 3, §3, the triple constraint). That's the **sponsor** — Priya, for Atlas. If you can't name your sponsor in one second, that's not a detail to fix later; it's a gap that will surface at the worst possible moment, usually when you need a fast decision and discover nobody actually has the authority to make it. Confirm the sponsor explicitly in the charter (Week 1) and re-confirm it here — sponsors change, especially on projects that outlive a reorg.

## 5. The silent blocker — power without visible interest

The single most useful thing this lecture teaches is spotting **silent blockers**: people scoring high on power and low on interest, *today*. They are not currently a problem. They become one the moment something changes — a delay threatens a date they care about, a scope cut affects their team, a cost overrun crosses their threshold — and they react from zero context, often more harshly than if they'd been kept warm the whole time.

Amara Osei is Atlas's clearest silent blocker. She has real power (sales messaging, executive air cover, and — because Sales talks to the exact accounts Atlas is meant to retain — real credibility if she decides to publicly doubt the project). She has shown almost no visible interest so far. That's not evidence she doesn't care; it's evidence nobody has proactively looped her in. The Week 6 risk register would flag this the same way it flags a flaky API: **probability** she becomes actively opposed if surprised (moderate — the sharing-API risk could genuinely slip the date she'd care about), **impact** if she does (high — a VP publicly questioning a project to the exact customers it's meant to retain is close to worst-case). The response, per Week 6's response-strategy framework, is **mitigate**: proactively engage her *before* she needs to ask, not react after she's upset.

```sql
-- surface every high-power, low-interest stakeholder before they become a problem
SELECT name, role, power, interest
FROM stakeholders
WHERE silent_blocker_risk = TRUE
ORDER BY power DESC;
```

## 6. Writing an engagement strategy, not just a label

A quadrant label alone ("Amara is Keep Satisfied") doesn't tell you what to actually do Monday morning. A real engagement strategy is one sentence, specific enough to act on:

> **Priya (Manage Closely):** Weekly 1:1, full detail including risk register and burn — she's the decision-maker on any trade-off, so she needs the real picture, not a summary.
>
> **Amara (Keep Satisfied, silent blocker risk):** Proactive biweekly two-line email — "here's where Atlas stands, here's the one thing that could affect the renewal-account timeline" — *before* she has to ask, so if the sharing-API risk does materialize, it's not the first thing she's ever heard about Atlas.
>
> **Jamie (Keep Informed):** Weekly async update in a shared Customer Success channel, plain language, no jargon — she's translating this for real customers and needs facts she can repeat accurately, not internal engineering detail.
>
> **Derek (Monitor):** Included in the sprint-end demo invite; no dedicated 1:1 unless Design Systems becomes a bottleneck.

Notice the pattern: **Manage Closely gets depth, Keep Satisfied gets brevity and proactivity, Keep Informed gets honesty and plain language, Monitor gets inclusion without demand.** Lecture 2 turns each of these one-liners into a full row in the `comms_plan` table — channel, cadence, and owner, ready to actually run.

## 7. Re-mapping — this isn't a one-time diagram

A power/interest grid drawn once at kickoff and never revisited is already stale by week 3. Power and interest both move: a stakeholder's power changes when org charts change; interest changes when something starts affecting them directly (a slip that touches their date, a scope cut that touches their team). Re-score the table whenever something material changes — a new dependency owner shows up, a sponsor reorg happens, a stakeholder starts asking questions they never asked before. That's exactly why this lives in a table you can `UPDATE`, not a slide you'd have to redraw.

## 8. Check yourself

- What's the difference between power and interest, and why are they scored independently rather than combined into one number?
- Which quadrant gets the most 1:1 time, and which gets the lightest touch?
- Why is a "Keep Satisfied" stakeholder more dangerous to neglect than a "Monitor" stakeholder?
- Define "silent blocker" in one sentence, using Amara Osei as the example.
- Why must every project have exactly one identifiable sponsor, and what specific question should they always be able to answer?
- A stakeholder's interest score jumps from 2 to 5 mid-project. What should you do to your engagement strategy, and how soon?

If those are automatic, Lecture 2 turns this map into an actual communication plan — audience, message, channel, cadence — and teaches you to write status reports people trust.

## Further reading

- **PMI — "Stakeholder Engagement" overview:** <https://www.pmi.org/learning/library/stakeholder-engagement-effective-project-management-6675>
- **Mendelow's Power/Interest Grid (original academic framing):** <https://www.mindtools.com/aol0rms/stakeholder-analysis>
- **Atlassian — "Stakeholder management":** <https://www.atlassian.com/agile/project-management/stakeholder-management>
