# Lecture 2 — Mapping Dependencies

> **Duration:** ~2 hours. **Outcome:** You can find and write down every real dependency behind a piece of work — inside your team and across teams — explain why coordination cost, not raw task duration, is what actually kills schedules, and know how to reduce coupling instead of just tracking it.

A dependency is simple to define and easy to underestimate: **task or team A cannot finish until task or team B delivers something A needs.** Every project has them. What separates a project that handles dependencies well from one that gets blindsided isn't the number of dependencies — complex work always has several — it's whether they were **written down and made visible before they blocked anyone**, versus discovered the week they start hurting.

## 1. Two kinds of dependency: internal and cross-team

**Internal dependencies** live inside your own team. Recall from Lecture 1: two of Marcus's engineers built overlapping comment-threading logic because nobody had mapped that their two stories touched the same module. That's an internal dependency that was never made visible — not a technical failure, a *visibility* failure. Internal dependencies are usually the easiest to fix once you see them: a five-minute conversation in sprint planning ("these two stories both touch `CommentThread.tsx` — sequence them, or split explicitly who owns what") prevents the whole class of problem.

**Cross-team dependencies** are the harder, higher-stakes kind: Atlas needs something from a team that doesn't report to you, doesn't share your sprint calendar, and has its own priorities that may not obviously include "help Atlas." Two live examples on Atlas right now:

- **Platform team** owns the authentication and permissions service. Atlas's sharing feature needs a new capability — workspace-scoped roles instead of only user-scoped roles — that Platform hasn't built yet. Platform has its own roadmap and its own VP; Atlas is not automatically their top priority.
- **Design Systems team** owns Northlight's shared component library. Atlas's presence UI needs a new "avatar stack" component (overlapping circular avatars showing who's viewing a workspace) that doesn't exist yet in the library, and Design Systems has a backlog of its own requests from four other product teams.

The distinction matters because the *fix* is different. Internal dependencies are solved by better sequencing and communication within a team you already run. Cross-team dependencies require **negotiation, escalation, and sometimes a fallback plan**, because you don't control the other team's schedule — only your relationship with it and your leverage as a PM.

## 2. Making dependencies visible: the dependency table

Just like risks, dependencies are only useful once they're written down somewhere queryable, not held in your head or scattered across Slack DMs with three different team leads. You created the `dependencies` table in this week's setup. Populate it with Atlas's real cross-team dependencies:

```sql
INSERT INTO dependencies
    (dependency_id, depends_on_team, needed_by_team, what_is_needed,
     needed_by_date, status, coordination_owner)
VALUES
(1, 'Platform', 'Atlas', 'Workspace-scoped permission roles API',
    '2026-08-03', 'not_started', 'You (PM)'),
(2, 'Design Systems', 'Atlas', 'Avatar-stack UI component',
    '2026-07-27', 'in_progress', 'You (PM)'),
(3, 'Atlas', 'Data/Analytics', 'Workspace activity event schema (for adoption dashboards)',
    '2026-08-10', 'not_started', 'Marcus Webb'),
(4, 'Pulse.io (vendor)', 'Atlas', 'Stable production API credentials + SLA',
    '2026-07-24', 'in_progress', 'Marcus Webb');
```

Notice row 3: **dependencies run both directions.** Atlas isn't only a team waiting on others — the Data/Analytics team is waiting on *Atlas* to emit a stable event schema so they can build adoption dashboards. If you only track what's blocking you and never track what you're blocking, you'll be the surprise cross-team dependency on someone else's register — the exact failure mode you're trying to prevent for yourself.

Now the table earns its keep the same way the risk register did:

```sql
-- Everything Atlas is waiting on, most urgent first
SELECT what_is_needed, depends_on_team, needed_by_date, status
FROM dependencies
WHERE needed_by_team = 'Atlas'
ORDER BY needed_by_date ASC;
```

```sql
-- Everything Atlas owes to someone else — don't be the blocker
SELECT what_is_needed, needed_by_team, needed_by_date, status
FROM dependencies
WHERE depends_on_team = 'Atlas'
ORDER BY needed_by_date ASC;
```

```sql
-- Anything not_started with a needed-by date inside the next 2 weeks — your escalation list
SELECT what_is_needed, depends_on_team, needed_by_date, coordination_owner
FROM dependencies
WHERE needed_by_team = 'Atlas'
  AND status = 'not_started'
  AND needed_by_date <= CURRENT_DATE + INTERVAL '14 days';
```

*(SQLite note: `CURRENT_DATE + INTERVAL '14 days'` is Postgres syntax. In SQLite, use `date('now', '+14 days')` in the comparison instead.)*

## 3. Tracing chains: querying beyond one hop

A single row in `dependencies` only tells you about one link. The real danger, as §6 below covers, is a **chain** — Atlas waits on Design Systems, who waits on Brand, who waits on Legal. To see a chain in your data, not just in your head, add a self-referencing column so one dependency can point at the one blocking *it*:

```sql
ALTER TABLE dependencies
ADD COLUMN blocked_by_dependency_id INTEGER REFERENCES dependencies(dependency_id);
```

Now a four-hop chain is four ordinary rows, each pointing at the row blocking it:

```sql
INSERT INTO dependencies
    (dependency_id, depends_on_team, needed_by_team, what_is_needed,
     needed_by_date, status, coordination_owner, blocked_by_dependency_id)
VALUES
(5, 'Design Systems', 'Atlas', 'Avatar-stack UI component', '2026-07-27', 'blocked', 'You (PM)', 6),
(6, 'Brand', 'Design Systems', 'Sign-off on new avatar illustration style', '2026-07-20', 'blocked', 'Design Systems lead', 7),
(7, 'Legal', 'Brand', 'Trademark check on new secondary brand color', '2026-07-15', 'blocked', 'Brand lead', NULL);
```

A recursive query walks the whole chain from any starting point, which is exactly what you'd run the moment you suspect a dependency is more tangled than it first looks:

```sql
WITH RECURSIVE chain AS (
    SELECT dependency_id, depends_on_team, needed_by_team, what_is_needed,
           status, blocked_by_dependency_id, 1 AS hop
    FROM dependencies
    WHERE dependency_id = 5                 -- start from Atlas's need
    UNION ALL
    SELECT d.dependency_id, d.depends_on_team, d.needed_by_team, d.what_is_needed,
           d.status, d.blocked_by_dependency_id, chain.hop + 1
    FROM dependencies d
    JOIN chain ON d.dependency_id = chain.blocked_by_dependency_id
)
SELECT hop, depends_on_team, needed_by_team, what_is_needed, status
FROM chain
ORDER BY hop;
```

This returns all four hops in order, with a `hop` number showing how far removed each link is from Atlas's original need — the single most useful diagnostic when you suspect (Lecture 2's danger case) that a blocker is hiding several layers deep. Challenge 2 this week has you build and query exactly this structure for a real four-hop scenario.

## 4. Coordination cost: why dependencies cost more than they look like

Here's the part that surprises new PMs: a cross-team dependency doesn't just cost you the *waiting time*. It costs **coordination overhead** that's easy to miss until you total it up:

- **Discovery cost** — time spent figuring out the dependency exists at all, and exactly which team and which person owns it. (Atlas's Platform dependency wasn't identified until Sprint 4, three sprints after the sharing feature's design assumed it.)
- **Alignment cost** — time spent getting the other team to actually understand and prioritize the ask, which usually means a meeting, a written request, and often a re-explanation once their own priorities shift.
- **Interface risk** — even once delivered, what Platform ships might not exactly match what Atlas assumed. A workspace-permissions API that returns roles in a different shape than expected costs integration rework that wouldn't exist if the same team had built both sides.
- **Schedule coupling** — Atlas's date is now partially a function of Platform's date, and Platform doesn't feel that pressure the way Atlas's own team does, because it's not blocking *their* release.

A rule of thumb worth internalizing: **a task that takes one team three days can easily take six days once you add a cross-team handoff on either side** — not because the work grew, but because discovery, alignment, and interface mismatch are real time costs that don't show up in either team's estimate. This is exactly why Lecture 3's critical-path math treats cross-team dependency tasks with real caution: they're the tasks most likely to run long, and the ones a naive schedule most underestimates.

Put numbers on it for Atlas's Platform dependency, and the gap becomes concrete:

| Cost component | Estimated days |
|---|:-:|
| Marcus's engineering estimate for "build workspace-scoped permissions," if Atlas's own team owned it end to end | 3d |
| + Discovery: time until anyone confirmed Platform, not Atlas, owns this capability | 2d (already spent, Sprint 4) |
| + Alignment: getting a real commitment onto Platform's sprint, not just verbal interest | ~3d of PM time, spread over a week of follow-ups |
| + Interface risk: expected rework once the delivered shape doesn't exactly match Atlas's assumption | 1–2d (historical average for first-time cross-team API integrations at Northlight) |
| **Realistic total exposure** | **~9–10d**, roughly **3x** the raw engineering estimate |

Nobody wrote "10 days" on a sprint board — the 3-day estimate is the one that got planned against, and the other 6–7 days show up later as an unexplained slip. Tracking coordination cost explicitly, even roughly, is what keeps that gap from being a surprise in Sprint 7.

## 5. Reducing coupling: the actual fix, not just tracking

Mapping dependencies is necessary but it's not sufficient — a beautifully documented, still-blocking dependency has not actually helped you. The deeper move is **reducing coupling**: changing the plan so fewer things depend on other things in the first place.

Three concrete moves, applied to Atlas's real dependencies:

**a) Sequence around it, don't wait on it.** The Platform permissions API blocks the *sharing* feature specifically, not the whole release. If comments and basic workspace creation don't need workspace-scoped permissions (they can ship with simpler, user-level access first), you can **resequence the backlog** so Atlas ships value while Platform's work catches up, instead of the whole team idling on one blocked dependency. This is a direct callback to Week 3's slicing — a dependency on one thin vertical slice doesn't have to block every other slice.

**b) Build a fallback, not just a wait.** For the Design Systems avatar-stack component: if Design Systems slips past the needed-by date, Atlas's engineers could build a minimal, unstyled version themselves and swap in the polished component later without changing the underlying data model. This converts a hard blocker into a soft one — worse UI temporarily, but not a stalled feature. Document the fallback *before* you need it, as part of the dependency row's plan, the same way Lecture 1 asked for a response plan on every risk.

**c) Own the interface, not just the request.** Instead of "Platform, please build us permissions," a stronger move is proposing the actual API shape Atlas needs and getting Platform to agree to *that contract* early — even before they've built the implementation. This shrinks interface risk (the third coordination cost above) because both sides agreed on the shape weeks before integration, instead of discovering a mismatch the week you try to wire it up.

None of these make the dependency disappear. They shrink its blast radius — which is the realistic goal. You will never run a project, especially a technology project touching more than one team, with zero dependencies. The job is making sure none of them silently become the reason you miss a date.

## 6. A dependency chain, and why chains are the dangerous case

A single dependency (Atlas waits on Platform) is manageable — you can see it, you can escalate it, you know exactly who to talk to. The dangerous case is a **chain**: Atlas waits on Design Systems, who is waiting on the Brand team for logo/illustration approval on the new avatar assets, who is waiting on Legal for a trademark check on a redesigned icon set. Now a delay four hops away, in a team you've never spoken to, quietly threatens your release date — and nobody on that chain necessarily knows Atlas exists, let alone that it's waiting on them.

Chains are where dependency mapping stops being a nice-to-have and becomes essential: **you cannot manage a risk you can't see**, and a four-hop chain is invisible by default unless someone — usually the PM at the end of the chain who actually has a date on the line — traces it explicitly and asks at every hop, "what's blocking *you*?" Challenge 2 this week gives you exactly this scenario to untangle.

## 7. Check yourself

- Write down one real internal dependency and one real cross-team dependency from a project you've worked on (school, work, or personal) — even if nobody called it that at the time.
- Why does a cross-team dependency typically cost more than its raw task duration suggests? Name the four cost categories from §3.
- Give one concrete example of "reducing coupling" (not just tracking a dependency) applied to a project you know.
- Why is it important for Atlas to track what it *owes* other teams, not just what it's *waiting on*?
- What makes a dependency chain more dangerous than a single dependency, even if every individual link looks manageable?
- What does the `blocked_by_dependency_id` self-reference buy you that a plain list of independent dependency rows doesn't?

If those are automatic, Lecture 3 takes the schedule these risks and dependencies sit inside of, and shows you exactly where a slip costs you a launch date and where it doesn't — the critical path.

## Further reading

- **PMI — "Managing Project Dependencies":** <https://www.pmi.org/learning/library/managing-inter-project-dependencies-6046>
- **Atlassian — "Cross-team dependencies in Agile":** <https://www.atlassian.com/agile/agile-at-scale/cross-team-collaboration>
- **Martin Fowler — "Reducing Coupling":** <https://martinfowler.com/ieeeSoftware/coupling.pdf>
