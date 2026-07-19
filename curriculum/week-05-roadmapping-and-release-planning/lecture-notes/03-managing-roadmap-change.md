# Lecture 3 — Managing Roadmap Change

> **Duration:** ~2 hours. **Outcome:** You can decide whether a slip warrants a re-plan, recompute a release plan against reduced capacity using SQL/Python, write an early slip-communication message, and present the same roadmap truthfully at two different altitudes — exec and delivery — without contradiction.

Sprint 2 review. Marcus flags it in the retro: Advanced Permissions & Roles — item 2 in the release plan, 30 points, supposed to close out R1 — is not close to done. The legacy auth system it has to integrate with turns out to store role data in a format nobody documented, and untangling it is going to take **two more sprints than planned.** This is not a hypothetical for this lecture; it's the single most common event in release planning: something you sized honestly, in good faith, turns out to be bigger once the team is actually inside it. Weeks 1–4 gave you the muscle to catch this early (Week 1's monitoring & control) and size it accurately going forward (Week 4). This lecture is about what happens *next*, at roadmap altitude, when catching it early isn't enough to make the problem disappear.

## 1. Change is not the failure — mishandled change is

A roadmap that never changes either describes a project with zero uncertainty (rare) or is being kept artificially stable by someone quietly absorbing the difference through unpaid overtime, cut corners, or silence (common, and worse than the change itself). **The failure that actually damages trust isn't the slip — it's a slip discovered late, communicated badly, or not communicated at all.** Lecture 1 already drew this distinction between roadmap, plan, and promise; this lecture is about what to *do*, concretely, the moment the plan and reality diverge.

## 2. Deciding whether a slip warrants a re-plan

Not every slip needs a roadmap-level response. Use a simple threshold test before escalating:

| Signal | Absorb quietly within the release | Re-plan the release/roadmap |
|---|---|---|
| **Size of the slip** | A few points, inside the buffer already reserved (Lecture 2, §3) | Large enough to eat into or exceed the reserved buffer |
| **Downstream dependents** | Nothing else in the plan depends on the late item | Other items (here: SOC 2) depend on the late item and are now also at risk |
| **External commitments** | No fixed date is threatened | A fixed date (here: September 13) is now genuinely at risk |
| **Visibility already given** | Stakeholders already expect some wobble in this release | Stakeholders were told this release was high-confidence |

Advanced Permissions slipping two sprints fails **every** column: it's far larger than the 6-point buffer R1 had left over, SOC 2 depends directly on it, SOC 2's date is externally fixed and cannot move, and R1 was presented to Priya as a fixed-scope-but-high-confidence release. This is a re-plan, not a quiet absorption — and recognizing that quickly, rather than hoping it resolves itself, is the whole point of the test.

## 3. Recomputing the plan against reduced capacity

If Advanced Permissions now finishes at the end of Sprint 4 instead of Sprint 2, everything downstream needs to be resequenced against what capacity actually remains. Recompute it explicitly rather than guessing:

```python
import pandas as pd

sprints = pd.DataFrame({
    "sprint_num": [1, 2, 3, 4, 5, 6],
    "capacity":   [19, 19, 19, 19, 19, 19],
})

backlog = pd.DataFrame({
    "title":         ["Team Workspaces hardening", "Advanced Permissions & Roles",
                       "SOC 2 audit logging", "Data Export API (v1)",
                       "Onboarding revamp", "Presence indicators", "Dark mode"],
    "points":        [8, 30, 20, 26, 24, 14, 8],
    "priority_rank": [1, 2, 3, 4, 5, 6, 7],
})

# Reality check: Permissions actually consumed sprints 1-4 (its own 30 points
# plus 2 sprints' worth of capacity burned on the legacy-auth rework: ~38 pts
# of capacity spent on a 30-pt item — call the extra 8 pts "rework overhead").
consumed_by_sprint4 = 8 + 30 + 8          # hardening + permissions + rework overhead
capacity_by_sprint4 = sprints.loc[sprints.sprint_num <= 4, "capacity"].sum()
remaining_capacity  = sprints["capacity"].sum() - consumed_by_sprint4

print(f"Capacity spent through Sprint 4: {consumed_by_sprint4}")
print(f"Q3 capacity remaining for Sprints 5-6: {remaining_capacity}")
```

```
Capacity spent through Sprint 4: 46
Q3 capacity remaining for Sprints 5-6: 68
```

Only **68 points remain** for everything still unshipped — SOC 2 (20), Data Export API (26), Onboarding (24), plus whatever was already deferred. That's 70 points of *originally in-scope* work against 68 points of *actual* remaining capacity, before even considering Presence Indicators or Dark Mode. The gap is small (2 points) but real, and SOC 2's hard date has now moved from "comfortably inside R2" to "the very next thing that must start, with almost no room for another surprise."

## 4. Protecting the fixed date first

With SOC 2's audit window immovable, it gets first claim on the remaining capacity — not because it's intrinsically more important than Data Export or Onboarding, but because it's the one item on the list with zero flexibility on *when*. Everything else has flexibility on *when*; only scope and sequencing are still negotiable for those. The re-plan:

```python
remaining_items = backlog[backlog.title.isin(
    ["SOC 2 audit logging", "Data Export API (v1)", "Onboarding revamp"]
)].copy()

remaining_items["forced_priority"] = [1, 2, 3]   # SOC 2 first — hard date wins
remaining_items = remaining_items.sort_values("forced_priority")
remaining_items["cumulative"] = remaining_items["points"].cumsum()
print(remaining_items[["title", "points", "cumulative"]])
```

```
                  title  points  cumulative
0  SOC 2 audit logging      20          20
1 Data Export API (v1)      26          46   <- exceeds 68? no, fits (46 <= 68)
2    Onboarding revamp      24          70   <- 70 > 68, over by 2
```

SOC 2 and the *full* Data Export API both fit inside the 68 remaining points. Onboarding, at full scope, pushes 2 points over. Two honest options, and only one of them is a decision instead of a hope: **(a)** trim 2 points off Onboarding's scope (drop one small guided-setup polish story), or **(b)** accept Onboarding slips fully into Q4. Either is defensible — what's not defensible is pretending the 2-point gap doesn't exist and hoping the team just runs faster.

## 5. Communicating the slip early

Week 1 taught "surface bad news early and small." Applied here, at Sprint 2 (the moment the slip is *known*, not the moment SOC 2's date arrives and it's undeniable):

> **To: Priya. Subject: Advanced Permissions is running ~2 sprints long — here's the re-plan.**
>
> Short version: Advanced Permissions hit an undocumented legacy auth format; it needs through Sprint 4 instead of Sprint 2. That's inside our control, not a surprise from outside — but it does affect the rest of Q3, so flagging it now while we still have room to choose, not in September when we won't.
>
> What stays fixed: SOC 2's audit date (Sept 13) is untouched — it now has first claim on our remaining capacity and I'm confident we hit it.
> What's still on track: Data Export API v1 ships as planned in R3.
> What's at risk: Onboarding revamp is ~2 points over the capacity we have left. I'm recommending we trim one small scope item from it rather than let it slip whole into Q4 — details attached. Want to align before I tell Elena?
>
> No action needed from you unless you disagree with protecting SOC 2's date over Onboarding's full scope — that's the one real tradeoff in this re-plan.

This message does everything Week 1's monitoring & control lecture described: it's early (Sprint 2, not Sprint 6), it's small (one clear ask), it names what's protected and what's at risk without burying the real decision, and it gives Priya an actual choice instead of just informing her of a fait accompli.

## 6. Logging the change

Just as Week 1's monitoring & control phase kept a change log for a single project, a roadmap needs one too — and it belongs in the same database as the backlog, not a side document nobody checks:

```sql
CREATE TABLE roadmap_change_log (
    change_id     SERIAL PRIMARY KEY,
    changed_at    DATE NOT NULL,
    item_title    TEXT NOT NULL,
    change_type   TEXT NOT NULL,   -- 'slip', 'scope_cut', 'reprioritized', 'added'
    description   TEXT NOT NULL,
    approved_by   TEXT NOT NULL
);

INSERT INTO roadmap_change_log (changed_at, item_title, change_type, description, approved_by)
VALUES
('2026-08-14', 'Advanced Permissions & Roles', 'slip',
 'Legacy auth format undocumented; +2 sprints. Detected Sprint 2 review.', 'Marcus (flagged), Priya (informed)'),
('2026-08-14', 'Onboarding revamp — guided setup', 'scope_cut',
 'Trimmed one polish story (2 pts) to protect SOC 2''s fixed Sept 13 date.', 'Priya');
```

A queryable change log answers, months later, the question every retrospective eventually asks — "why did this slip, and did we handle it well?" — with facts instead of memory.

## 7. Presenting to two audiences from one truth

The re-planned roadmap is *one* set of facts. It gets presented differently to Priya (exec altitude) and to Marcus's team (delivery altitude) — different detail, same truth, never contradicting each other:

| | Exec version (Priya, board) | Delivery version (Marcus's team) |
|---|---|---|
| **Unit of detail** | Themes and outcomes | Individual backlog items and sprints |
| **Dates shown** | Only for Now-bucket, fixed-date items (SOC 2's Sept 13) | Every item's target sprint |
| **Language for risk** | "Onboarding may trim scope to protect the SOC 2 date" | "Cut story: guided-setup tooltip polish (2 pts) — see backlog item #5b" |
| **Length** | Half a page | Full sprint-by-sprint table |
| **Purpose** | Confidence the right tradeoffs are being made | Actionable next-sprint plan |

The test for whether two versions are healthy or dishonest: **could Priya's half-page summary and Marcus's full sprint table both be shown to the same skeptical outsider, side by side, with no contradiction?** If yes, you've matched altitude correctly. If the exec version hides a risk the delivery version reveals, that's not simplification — that's two different stories, and it will surface exactly when it's most damaging to have two stories. Challenge 1 has you build both versions from this exact re-plan.

## 8. Check yourself

- Run the threshold test from §2 against a slip of your own choosing (real or invented) — does it warrant a re-plan or a quiet absorption? Justify using all four columns.
- Why does SOC 2 get first claim on the remaining 68 points, even though it isn't the highest-value item on the list?
- Rewrite the §5 slip message as if Priya were far less involved day-to-day and needed even more context — what would you add, and what would you still leave out?
- What's the actual test (§7) for whether an exec-altitude and delivery-altitude version of the same roadmap are honest versions of each other, rather than two different stories?
- A colleague says "just don't tell leadership about small slips, only big ones." Using this lecture's vocabulary, what's the flaw in "small" being decided *after* it becomes big?

This closes the loop on the whole week: build the roadmap (Lecture 1), plan the release under it (Lecture 2), and now keep both honest as reality intervenes (Lecture 3) — the same discipline, applied continuously, not just once at the start of a quarter.

## Further reading

- **Atlassian — "How to communicate a roadmap change":** <https://www.atlassian.com/agile/product-management/product-roadmaps>
- **Marty Cagan / SVPG — "The Trust Gap":** <https://www.svpg.com/the-trust-gap/>
- **PostgreSQL docs — `SERIAL` and audit-log table patterns:** <https://www.postgresql.org/docs/current/datatype-numeric.html#DATATYPE-SERIAL>
