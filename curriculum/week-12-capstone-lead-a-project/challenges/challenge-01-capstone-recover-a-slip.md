# Challenge 1 — Recover Your Capstone From a Mid-Delivery Slip

**Skill:** Diagnosing a real slip from flow data, running a forecast on it, and choosing and defending a recovery lever under realistic pressure — Lecture 2 §5–6 and Lecture 3 §1, applied to a scenario, not your own (yet-undecided) outcome.

**Estimated time:** 1.5 hours.

---

## The scenario

**Sam Okafor** is running a one-week capstone almost identical in shape to Jordan's TaskPing: a 13-item backlog, `must`-priority items making up 7 of them, targeting a Saturday ship date. It's Thursday morning, day 4. Sam's sponsor-stand-in (a course peer reviewer, **Devon Reyes**) is checking in, and Sam needs an honest answer, not a hopeful one.

Here is Sam's actual data. Load it into a scratch table — do **not** touch your own `capstone_items`.

```sql
CREATE TABLE IF NOT EXISTS scratch_slip_items (
    item_id        INTEGER PRIMARY KEY,
    item_key       TEXT    NOT NULL,
    title          TEXT    NOT NULL,
    priority       TEXT    NOT NULL,
    created_at     DATE    NOT NULL,
    started_at     DATE,
    done_at        DATE,
    current_status TEXT    NOT NULL
);

INSERT INTO scratch_slip_items VALUES
(1,'PD-01','Set up project skeleton','must','2026-06-08','2026-06-08','2026-06-08','Done'),
(2,'PD-02','Data model for diary entries','must','2026-06-08','2026-06-08','2026-06-09','Done'),
(3,'PD-03','Create-entry command','must','2026-06-08','2026-06-09','2026-06-09','Done'),
(4,'PD-04','List-entries command','must','2026-06-08','2026-06-09','2026-06-10','Done'),
(5,'PD-05','Search entries by keyword','must','2026-06-08','2026-06-10','2026-06-10','Done'),
(6,'PD-06','Third-party image-thumbnail library integration','must','2026-06-08','2026-06-10',NULL,'Blocked'),
(7,'PD-07','Export entries to Markdown','must','2026-06-08',NULL,NULL,'Ready'),
(8,'PD-08','README + install instructions','must','2026-06-08',NULL,NULL,'Backlog'),
(9,'PD-09','Tag entries by mood','should','2026-06-08',NULL,NULL,'Backlog'),
(10,'PD-10','Basic full-text search index','should','2026-06-08',NULL,NULL,'Backlog'),
(11,'PD-11','Encrypt entries at rest','should','2026-06-08',NULL,NULL,'Backlog'),
(12,'PD-12','Nightly backup script','could','2026-06-08',NULL,NULL,'Backlog'),
(13,'PD-13','Import from a competing app''s export format','could','2026-06-08',NULL,NULL,'Backlog');
```

Sam's risk register has this entry, logged Tuesday and never updated since:

```sql
CREATE TABLE IF NOT EXISTS scratch_slip_risks (
    risk_id INTEGER PRIMARY KEY, description TEXT, probability INTEGER, impact INTEGER, status TEXT, logged_at DATE
);

INSERT INTO scratch_slip_risks VALUES
(1, 'Third-party image-thumbnail library has undocumented breaking changes between the version in the tutorial and the current release', 4, 4, 'open', '2026-06-09');
```

---

## Part A — Diagnose (25 min)

1. Compute Sam's daily throughput from `scratch_slip_items` (Lecture 2, §5c). Note the gap: how many days show zero completions, and when did it start?
2. Query PD-06's status directly. How many days has it been `Blocked` as of Thursday (2026-06-11)?
3. In one paragraph, connect the risk register entry to the throughput data: is this a "the team is slower" story or a "one specific, previously-flagged risk materialized" story (Week 8's framing)? Defend your answer with the specific dates and rows, not a general impression.

## Part B — Forecast (25 min)

1. Using Sam's throughput series and a remaining-item count of 8 (7 `must` + PD-06 already counted, minus PD-01 through PD-05 done — recompute this yourself from the table, don't take 8 on faith), run the Monte Carlo simulation from Lecture 2, §5e.
2. Report p50/p85/p95 in days from Thursday, 2026-06-11.
3. Does the p85 land on or before Saturday, 2026-06-13? Show your work.

## Part C — Choose and defend a recovery lever (25 min)

You have the same three levers as Lecture 2, §6. For each, write one real sentence of what it would concretely mean for Sam's project — not the generic definition, the specific action:

- **Cut scope** — which item(s), specifically, and does cutting them still satisfy the charter's success criteria?
- **Extend the date** — to when, and is that actually available given Sam's real constraints (you don't know Sam's calendar — state your assumption explicitly)?
- **Increase throughput** — is this realistic here, or is it the "just work harder" trap Week 1 and Week 8 both warn against? Be specific about why.

Pick **one** and write a 150–200 word recommendation, as if you were advising Sam directly.

## Part D — Draft the hard message (15 min)

Write the actual message Sam should send Devon (the peer-reviewer stand-in for a sponsor), following Week 7's status-report and escalation format: what's true, what changed, what you're doing about it, and what you need (if anything) from Devon. It should be honest about the slip without being either defensive or catastrophizing — reread Week 7 Lecture 3 on escalation tone if you're unsure what that balance looks like.

---

## Expected outcome

A short write-up (`challenge-01-response.md`) with: the diagnosis (Part A), the forecast numbers and whether they clear Saturday (Part B), your chosen lever with its specific justification (Part C), and the actual message text (Part D).

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|---------------------|
| Diagnosis | 25% | Ties the specific blocked item and specific risk-register entry to the specific throughput gap — not a generic "things slowed down" |
| Forecast | 25% | Correct remaining-item count, correctly run simulation, honest read of whether p85 clears the deadline |
| Recovery lever | 30% | Specific, defensible, and consistent with the charter's success criteria — not a dodge ("I'd just work harder") |
| Stakeholder message | 20% | Honest, calm, specific — states the change and the plan without burying the bad news or overselling the recovery |
