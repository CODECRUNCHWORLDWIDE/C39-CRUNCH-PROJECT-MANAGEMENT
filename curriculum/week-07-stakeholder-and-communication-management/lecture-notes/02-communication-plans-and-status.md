# Lecture 2 — Communication Plans & Status

> **Duration:** ~2 hours. **Outcome:** You can build a communication plan that specifies audience, message, channel, and cadence for every stakeholder, and write a RAG status report that is honest, scannable in under a minute, and never buries a slip behind good news elsewhere in the report.

Lecture 1 told you *who* gets how much attention. This lecture is about *what you actually send them* — and the single skill that separates trusted PMs from ignored ones: status reporting that people believe, because it has never once told them things were fine when they weren't.

## 1. From map to plan: audience, message, channel, cadence

A communication plan turns each stakeholder's engagement strategy (Lecture 1, §6) into four concrete decisions:

- **Audience** — who, specifically. Not "leadership" — Priya, or Priya-and-Tom-together, or the whole exec team. Vague audiences produce vague messages.
- **Message focus** — what *this* audience needs, not everything you know. Priya needs trade-off-level detail; Jamie needs plain-language facts she can repeat to a customer; Tom needs burn and forecast, not sprint velocity.
- **Channel** — the medium that matches how this person actually consumes information. A person who reads email at 6am wants a digest, not a meeting invite; a person who skims Slack wants a short channel post, not a PDF attachment.
- **Cadence** — how often, and is it fixed-schedule or trigger-based. Some stakeholders need a steady weekly touchpoint; others (Tom, Amara — Keep Satisfied) need proactive contact *when something changes*, not noise on a fixed schedule they'll start ignoring.

Populate `comms_plan` from Lecture 1's engagement strategies:

```sql
INSERT INTO comms_plan
    (comms_id, stakeholder_id, message_focus, channel, cadence, owner, next_due)
VALUES
    (1, 1, 'Full status: scope/schedule/budget RAG, top risk, any decision needed', '1:1',              'weekly',      'PM', '2026-07-20'),
    (2, 2, 'Backlog/sprint detail, blockers affecting current sprint',              'standup + Slack',  'daily-ish',   'PM', '2026-07-19'),
    (3, 3, 'Technical risk detail, dependency status',                              '1:1',               'weekly',      'PM', '2026-07-20'),
    (4, 6, 'Two-line proactive update: overall status + anything touching renewal timing', 'email digest', 'biweekly + on-trigger', 'PM', '2026-07-24'),
    (5, 7, 'Burn vs. budget, forecast to completion',                               'exec status doc',  'monthly',     'PM', '2026-08-01'),
    (6, 8, 'Plain-language status the CS team can repeat to customers',             'Slack channel',     'weekly',      'PM', '2026-07-19'),
    (7, 5, 'Sprint-end demo invite; design-system dependency notes if relevant',    'demo invite',       'sprint-end',  'PM', '2026-07-25');
```

Query it as a to-do list — this is the actual value of putting it in a table instead of a static document:

```sql
-- who's overdue or due soon, sorted by urgency
SELECT s.name, s.quadrant, c.message_focus, c.channel, c.next_due
FROM comms_plan c
JOIN stakeholders s ON s.stakeholder_id = c.stakeholder_id
ORDER BY c.next_due ASC;
```

That single query replaces "did I remember to update Amara" with a fact you can check in five seconds. A communication plan you can't query is a communication plan you'll quietly stop following by week 4.

## 2. One message, many audiences — same facts, different framing

The underlying facts of Atlas's status are the same for everyone this week: the sharing-API risk from Week 6 has partially materialized (the sandbox outage extended past its trigger date), and the Platform dependency is two days behind. What changes per audience is **framing and depth**, never the underlying truth:

> **To Priya (Manage Closely):** "The sharing-API risk from the register materialized on schedule — sandbox is still degraded. Platform's delivery is 2 days behind their committed date. Net effect: sprint 7's sharing-permissions work is at risk of slipping into sprint 8's buffer. I have three options and want your call by Thursday — see the attached options." Full detail, real numbers, a decision requested.
>
> **To Amara (Keep Satisfied):** "Quick update: Atlas is tracking to plan overall; one technical dependency is running a couple of days behind and we're actively managing it. No change expected to the GA date at this point — I'll flag immediately if that changes." True, concise, proactive, and it tells her the one thing she actually cares about (does this affect the date) without the internal mechanics she didn't ask for.
>
> **To Jamie (Keep Informed):** "Atlas is still on track for the committed GA date. We hit a small technical snag this week that the engineering team is actively working through — nothing that changes what you can tell customers right now. I'll give you a heads-up the moment that changes." Plain language, no jargon, explicitly usable in a customer conversation.

All three are **true**. None of them lies by omission, and none of them dumps irrelevant detail on someone who didn't ask for it. That's the craft: the same underlying fact, honestly represented, shaped for what each specific person needs to do with it.

## 3. Honest RAG status — defining Green, Amber, and Red so they mean something

RAG (Red/Amber/Green) status only works if the colors mean the same thing every time you use them. Vague, mood-based RAG — "I feel good about this, call it Green" — is exactly what erodes trust, because a sponsor eventually learns your Green doesn't predict anything. Define each color against **observable criteria**, in advance:

| Status | Scope | Schedule | Budget |
|--------|-------|----------|--------|
| 🟢 **Green** | On track against charter scope; no unresolved change requests | Current sprint on pace; no critical-path task at risk | Burn tracking within 5% of forecast |
| 🟡 **Amber** | A change request is pending decision, or minor scope ambiguity exists | A critical-path task or dependency is at risk but a mitigation is in flight and has a real chance of holding the date | Burn tracking 5–15% over forecast, with a credible plan to recover |
| 🔴 **Red** | Scope has slipped without an approved change, or a needed decision is overdue | A critical-path task has already slipped, or mitigation has failed and the committed date is no longer credible | Burn is 15%+ over forecast with no credible recovery plan |

With this definition, Atlas's current status isn't a feeling — it's a lookup. The sharing-API risk materializing plus a 2-day Platform slip against a critical-path task (Week 6, Lecture 3) with an active, credible mitigation (buffer absorption, per Week 6's slack analysis) is **Amber on schedule** — not Green (something real did happen), not Red (the date is still credible and a mitigation is in flight). Insert it:

```sql
INSERT INTO status_log
    (status_id, report_date, overall_rag, scope_rag, schedule_rag, budget_rag,
     headline, top_risk, decision_needed, author)
VALUES
    (7, '2026-07-18', 'amber', 'green', 'amber', 'green',
     'On track overall; sharing-API sandbox outage is eating into sprint 8 buffer — GA date still credible but margin is thinner than last week.',
     'Sharing-API sandbox outage (risk_id 3 in the register) — mitigation in flight, buffer absorbing it so far',
     'None yet — will need a scope or date call by Thursday if the sandbox is still down',
     'PM');
```

**The discipline that matters most:** never let a Green headline sit next to a Red or Amber sub-item buried three paragraphs down. If any dimension is Amber or Red, the **overall** status and the **headline** must say so, plainly, first. A status report where the top line says "on track" and paragraph four quietly mentions a missed critical-path task is worse than no report at all — it trains the reader to stop trusting your top line, which means the *next* time you genuinely are on track, nobody believes you either.

## 4. The one-glance summary

A status report competes with a sponsor's inbox, not with their full attention. Structure every report so the first five seconds tell the whole story, before anyone reads a single sentence of detail:

```
PROJECT ATLAS — Status: 2026-07-18
Overall: 🟡 AMBER   Scope: 🟢   Schedule: 🟡   Budget: 🟢

HEADLINE: On track overall; sharing-API sandbox outage is eating into
sprint 8 buffer — GA date still credible but margin is thinner than
last week.

TOP RISK: Sharing-API sandbox outage — mitigation in flight.
DECISION NEEDED: None yet; possible scope/date call by Thursday if
the outage continues.
```

Then, below that four-line block, the detail: what shipped this week, what's planned next, the full risk-register excerpt, dependency status, burn numbers — for whoever wants to keep reading. But the top block alone must be a complete, honest, standalone answer to "how's it going," because that's the only part most readers will ever see. Query it straight out of the table:

```sql
SELECT report_date, overall_rag, scope_rag, schedule_rag, budget_rag, headline, top_risk, decision_needed
FROM status_log
ORDER BY report_date DESC
LIMIT 1;
```

## 5. Never bury a slip

The single most trust-destroying habit in status reporting is **burying bad news inside good news** — leading with three green wins and mentioning the actual problem as a throwaway line at the end, hoping it won't register. It always registers, just later and worse: the reader eventually connects the dots, realizes the report was structured to soften bad news rather than communicate it, and re-reads every past "Green" with suspicion.

The fix is structural, not moral — you don't need to *try harder* to be honest, you need a **template that makes burying things awkward**. The one-glance block in §4 forces this: if schedule is Amber, the top line says Amber, full stop, before any good news gets a chance to overshadow it. Compare:

> **Buries it:** "Great sprint! Shipped workspace creation and teammate invites ahead of schedule. Comments UI is polished and ready for review. One small thing — Platform's dependency slipped a couple days, should be fine. Demo Friday!"
>
> **Doesn't bury it:** "🟡 AMBER — schedule. Platform's sharing-API dependency slipped 2 days and is eating into our sprint 8 buffer; still on track for GA but margin is thinner. Also shipped: workspace creation and teammate invites, ahead of schedule; comments UI ready for review. Demo Friday."

Same facts, same actual sprint. The second version leads with the thing that changed the risk picture, gives it a real sentence instead of a throwaway clause, and *then* shares the wins — which land better, not worse, because the reader trusts the report enough to actually enjoy the good news instead of scanning for what's being hidden.

## 6. Check yourself

- Name the four components of a communication plan row, and give one sentence on why each stakeholder might need a different value for each.
- Priya and Amara get different messages about the same underlying Atlas status this week. What's different, and what must stay identical between the two?
- Define Green, Amber, and Red for the *schedule* dimension in your own words, using observable criteria, not a feeling.
- Why is "overall Green with one Red sub-item mentioned in paragraph four" worse for trust than an honest overall Amber?
- Rewrite the "buries it" status excerpt in §5 so it doesn't bury the slip, keeping every fact but changing the order and the top line.
- What single query would tell you, right now, which stakeholders are overdue for their next scheduled update?

If those are automatic, Lecture 3 covers what to do when the news is worse than Amber — escalating cleanly, running a change request, and managing two stakeholders who want opposite things.

## Further reading

- **PMI — "Effective Project Status Reporting":** <https://www.pmi.org/learning/library/effective-project-status-reporting-6480>
- **Atlassian — "How to write a project status report":** <https://www.atlassian.com/work-management/project-management/status-report>
- **Harvard Business Review — "How to Say What You Mean" (on framing difficult news):** <https://hbr.org/2022/01/how-to-say-what-you-mean>
