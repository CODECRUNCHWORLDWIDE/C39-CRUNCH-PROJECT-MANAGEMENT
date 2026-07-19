# Lecture 3 — Escalation & Change Management

> **Duration:** ~2 hours. **Outcome:** You can write an escalation that gets a fast decision instead of a shrug, run a lightweight change-request process that keeps scope decisions explicit instead of silent, and sit two stakeholders who want opposite things across the table and get to a real decision without dodging the conflict.

Lecture 2 taught you to report status honestly. This lecture teaches you what to do when honesty isn't enough by itself — when you're actually stuck, when someone wants to change scope, or when two people who both have real power want different things. All three situations share one root cause: a decision needs to be made that you can't or shouldn't make alone, and the whole skill is getting that decision made *cleanly* instead of it happening by drift, avoidance, or whoever's loudest.

## 1. Escalation is a request, not a complaint

An escalation is you handing a decision to someone with the authority to make it — not venting a problem and hoping someone else deals with it. The difference is structural: a complaint describes a problem and stops; a real escalation describes the problem, its impact, and **exactly what you want the reader to do**.

**Weak escalation (a complaint):**

> "Heads up, Platform's dependency for the sharing API is still not done and it's really stressing the team out. Just wanted to flag it."

This tells Priya something is wrong but asks nothing of her. She either has to do the work of figuring out what you need — which most sponsors won't reliably do — or nothing happens and the problem just sits there, unresolved, having cost you the credibility of "flagging" it without it going anywhere.

**Strong escalation (a real ask):**

> "The Platform team's sharing-API dependency (originally due 2026-07-14) is now 3 days late; their new estimate is 2026-07-21. This dependency sits on Atlas's critical path (Week 6 analysis) — every day it slips, sprint 8's buffer shrinks by a day, and we have 4 days of buffer left. **I need one of two things from you by Wednesday:** (1) your help getting Sofia's team to prioritize this over their other work this week, or (2) your sign-off to cut real-time presence indicators from this release (charter's out-of-scope candidate list) to protect the GA date if the dependency isn't resolved by Friday. I recommend (1) first, with (2) as the fallback if that doesn't move the needle by Friday."

Same underlying problem, completely different outcome. This version gives Priya a fact, a quantified impact, a deadline, two concrete options, and a recommendation — she can say "yes, option 1" in one sentence and the decision is made. That's what escalation is *for*.

## 2. The four-part escalation template

Every strong escalation has these four parts, in this order:

1. **The fact** — what happened, with numbers and dates, not adjectives. "3 days late" not "really behind."
2. **The impact** — what this actually threatens, tied to something the reader cares about (a date, a budget line, a commitment already made externally). Connect it to the charter's constraints or success criteria (Week 1) whenever you can — impact lands harder when it's tied to something the reader already agreed matters.
3. **The ask** — the specific decision or action you need, and by when. Never leave this implicit.
4. **A recommendation** — your view on which option you'd pick, even though it's their call. This isn't overstepping; it's doing the thinking so the reader doesn't have to do it cold, and it signals you've already worked the problem rather than just discovered it.

A useful gut-check before sending: **could the reader respond in one sentence?** If your escalation requires them to ask three clarifying questions before they can decide anything, it's not finished — you did too little of the thinking.

## 3. When to escalate vs. when to handle it yourself

Not every problem is an escalation. The test: **can you resolve this within your own authority and the charter's existing constraints?** If yes, handle it and just report it in the next status update (Lecture 2) — escalating things you could have solved yourself trains people to stop taking your escalations seriously, the same way a fire alarm nobody trusts stops getting evacuations. If the answer requires spending money you don't control, changing a date the charter fixed, changing scope beyond what you're authorized to trade, or resolving a conflict between two people who outrank you, that's a real escalation — waiting too long on those is just as damaging as escalating too often, because by the time you finally raise it, the options have narrowed and the impact has grown.

Two failure modes to watch for in yourself:

- **Escalating too much** — every minor blocker becomes a fire drill; sponsors start ignoring your messages, which is fatal the one time you have a real one.
- **Escalating too little** — you sit on a real blocker hoping it resolves itself, and by the time you raise it, the runway to fix it is gone and the only options left are worse ones.

The Platform dependency escalation in §1 is a genuine escalation: it threatens the critical path, the fix requires either Sofia's prioritization (not yours to command) or a scope cut (not yours to unilaterally approve). A missing Jira label is not.

## 4. Running a lightweight, explicit change-request process

Scope drifts in one of two ways: loudly, through a formal request someone can evaluate, or quietly, through a Slack DM that turns into "sure, I'll just add that" with nobody weighing the trade-off. The second way is how the triple constraint (Week 1, Lecture 3, §3) gets silently violated — quality erodes because nobody ever *decided* to trade time or scope for the new ask, it just accreted.

The fix doesn't need to be heavyweight — a lightweight process beats no process, and a process people actually use beats a rigorous one they route around. One table, one rule: **any request that changes scope, date, or budget gets logged before it gets actioned**, even if the answer ends up being "yes, obviously, go ahead."

```sql
INSERT INTO change_requests
    (cr_id, title, requested_by, date_requested, description,
     impact_scope, impact_schedule, impact_cost, decision)
VALUES
    (1, 'Add bulk-invite (CSV upload) to teammate invites',
     'Amara Osei', '2026-07-16',
     'Sales wants bulk CSV invite for enterprise onboarding calls happening in August.',
     'New capability, not in original charter scope (Week 1 lists individual email invite only)',
     'Estimated 3–4 dev-days; would need to land by end of sprint 8 to matter for August calls',
     'No new budget requested; absorbed into existing sprint if approved',
     'pending');
```

Evaluate every change request against three questions, in this order:

1. **Does it serve the charter's objective?** (Week 1 — for Atlas, retaining the three at-risk enterprise accounts.) A request that doesn't serve the stated objective is easy to defer regardless of how reasonable it sounds in isolation.
2. **What does the triple constraint actually cost?** Every real "yes" moves something — more time, more scope cut elsewhere, or more resources. State it as a number, not a vibe: "3–4 dev-days, absorbed into sprint 8's buffer" or "3–4 dev-days, requiring either a 1-week date slip or cutting X."
3. **Who has the authority to say yes?** Per Week 1's decision-authority section — usually the sponsor for anything touching date or budget, the product owner for scope trades within an already-approved backlog.

Once decided, close the loop in the same table — don't let it become a decision nobody remembers making:

```sql
UPDATE change_requests
SET decision = 'approved', decided_by = 'Priya Chen', date_decided = '2026-07-19'
WHERE cr_id = 1;
```

Query the open ones as a standing to-do, exactly like the risk register and dependency map from Week 6:

```sql
SELECT cr_id, title, requested_by, date_requested, impact_schedule
FROM change_requests
WHERE decision = 'pending'
ORDER BY date_requested ASC;
```

The point of logging even "obvious yes" requests is that scope creep is never one dramatic decision — it's dozens of small, individually-reasonable "sure, why not" moments that nobody added up. A logged, decided change request is a **visible, chosen trade** (Week 1, Lecture 3, §3); an un-logged one is exactly the silent kind that erodes quality without anyone deciding it should.

## 5. Managing conflicting stakeholder priorities without drama

Sooner or later, two stakeholders with real power want different things. Amara wants bulk CSV invite added because it helps close August sales calls; Priya (and Marcus) are protecting the GA date and worry any new scope threatens it given the sharing-API risk already eating into buffer. Both are reasonable, from where each of them sits. Your job is **not** to privately pick a side, and it's **not** to avoid the conversation and let it fester — it's to get the actual decision-makers to a real decision, with full information, as fast as possible.

A four-step approach that works without turning into a political minefield:

1. **Separate the position from the underlying need.** Amara's position is "add bulk invite." Her underlying need is "enterprise prospects have a smooth onboarding story in August sales calls." Once you know the underlying need, you can sometimes find an option neither original position considered — maybe a manual CSV-to-individual-invite workaround Customer Success can run in August without touching engineering scope at all.
2. **Get the facts everyone needs on the table, neutrally.** Don't advocate for either side in how you present the facts — state the change request's real cost (§4) and the schedule risk already in play (Lecture 2) exactly as they are, to both parties, so nobody's arguing from a different set of facts.
3. **Name the actual trade-off explicitly, out loud, to the people with authority to make it.** "Adding this costs 3–4 dev-days against a buffer that's already thinner than planned because of the sharing-API risk. If we do it, either the date moves or something else gets cut. Priya, Amara — given that, what's the call?" This is the triple-constraint conversation from Week 1, run live, between two people who both have real stakes.
4. **Get a documented decision, not a vibe.** Whatever gets decided goes in the `change_requests` row (§4) with a name and a date attached. A decision that only exists as a memory of a hallway conversation will get relitigated the first time something goes wrong.

What you're explicitly *not* doing: silently siding with whichever stakeholder has more organizational power, silently siding with whichever one you personally agree with, or stalling in the hope the conflict resolves itself. All three erode trust with whichever side loses without a real hearing, and the third one — stalling — usually just lets the decision get made by default (the deadline passes, the answer becomes "no" without anyone choosing it), which serves nobody and looks, in hindsight, exactly like avoidance.

## 6. Check yourself

- Rewrite this complaint as a real escalation with all four parts: "The design system components still aren't ready and it's blocking the comments UI."
- What's the specific gut-check for whether an escalation is "finished" before you send it?
- Give one example of a problem you should handle yourself, and one you should escalate, and explain the difference in one sentence each.
- Why does even an "obvious yes" change request need to be logged before it's actioned?
- What are the three questions you evaluate every change request against, in order?
- In the Amara/Priya scenario, what is Amara's *position* and what is her likely *underlying need* — and why does that distinction matter?
- Name the one thing a PM should never do when two powerful stakeholders disagree, besides refusing to raise it at all.

If those are automatic, you're ready for this week's exercises, challenges, and the mini-project — building a real stakeholder map, communication plan, and status pack of your own.

## Further reading

- **PMI — "Escalation Management" overview:** <https://www.pmi.org/learning/library/escalation-management-project-success-6698>
- **Atlassian — "Change management process":** <https://www.atlassian.com/agile/project-management/change-management>
- **Harvard Business Review — "Managing Conflict Between Stakeholders":** <https://hbr.org/topic/subject/conflict-and-consensus>
