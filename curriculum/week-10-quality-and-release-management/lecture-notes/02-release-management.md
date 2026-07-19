# Lecture 2 — Release Management & Cutover

> **Duration:** ~2 hours. **Outcome:** You can build a release readiness checklist that covers more than code, choose between staged and big-bang rollout strategies for a given risk profile, run an actual go/no-go decision with named owners, and plan the communications that go with it. You'll load and query Atlas's real GA readiness checklist.

The DoD from Lecture 1 answers one question: *is the work itself finished?* Release management answers a different, broader one: *is the **organization** ready for this work to become real for customers, on a specific day, at a specific time?* A team can hit every line of its DoD and still run a disastrous release — because nobody told Support it was happening, because the database migration was never rehearsed against production-sized data, because the on-call engineer found out from a customer instead of a deploy notification. This lecture is about that second, wider readiness.

## 1. Release readiness is a different checklist than the DoD

The DoD is about the **work**. The release checklist is about the **event** — everything true only on release day, regardless of how good the code is:

| Category | What it covers | Example |
|---|---|---|
| Code freeze | A specific commit is tagged as the release candidate; nothing new lands without an explicit exception | `git tag v2026.04.29-ga` cut, no merges to `main` without sign-off after the tag |
| Environment | Staging matches production closely enough to trust; the deploy pipeline itself has been exercised | Staging soak test run for 48h under synthetic load |
| Data | Any migration is rehearsed against production-shaped data, and is reversible | Migration dry-run against a prod snapshot; rollback script tested |
| Monitoring | Dashboards and alerts exist for the new surface *before* it's live, not added after something breaks | SLA dashboard wired to the new webhook signature-verification metric |
| Comms | Internal and customer-facing communications are drafted, approved, and scheduled | Release notes reviewed by Jamie Okafor; internal Slack announcement drafted |
| Support | The people who'll field the first customer questions know what shipped and how to help | Support macros + FAQ published; Support briefed in a 15-minute sync |
| Rollback | A tested, specific backout plan exists (full detail in Lecture 3) | Rollback runbook reviewed and rehearsed |

Notice the DoD from Lecture 1 sits *inside* "code freeze" and partially "monitoring" — but the release checklist is strictly bigger. A release can be **DoD-complete and release-incomplete**: every line of code done, tested, and reviewed, but Support never briefed and no rollback plan written. That release will still go badly.

## 2. Atlas's real GA readiness checklist

```sql
INSERT INTO release_checklist (check_id, release_name, category, item, owner, status, due_date, notes) VALUES
(1,  'atlas-ga-2026-04-29', 'code freeze', 'Cut release candidate tag v2026.04.29-ga',                         'Marcus Diallo', 'done',        '2026-04-24', 'Tagged after Sprint 8 close'),
(2,  'atlas-ga-2026-04-29', 'code freeze', 'No merges to main without explicit exception sign-off',            'Marcus Diallo', 'in_progress', '2026-04-29', 'One exception pending: hotfix for ATLAS-131'),
(3,  'atlas-ga-2026-04-29', 'environment', '48h staging soak test at expected peak load',                      'Chris Okoye',   'blocked',     '2026-04-27', 'Blocked — soak test surfaces same failure as load test, see ATLAS-131'),
(4,  'atlas-ga-2026-04-29', 'environment', 'Deploy pipeline dry-run to production (no traffic cutover)',       'Omar Farouk',   'done',        '2026-04-26', 'Dry-run clean, 6m40s deploy time'),
(5,  'atlas-ga-2026-04-29', 'data',        'Schema migration dry-run against prod-shaped snapshot',            'Wei Zhang',     'done',        '2026-04-25', 'Additive-only migration, no destructive steps'),
(6,  'atlas-ga-2026-04-29', 'data',        'Migration rollback script tested',                                 'Wei Zhang',     'done',        '2026-04-25', 'Rollback script reverses cleanly in 40s'),
(7,  'atlas-ga-2026-04-29', 'monitoring',  'SLA dashboard wired to signature-verification success rate',       'Fatima Noor',   'done',        '2026-04-23', 'Alert threshold: <98% success rate over 5min'),
(8,  'atlas-ga-2026-04-29', 'monitoring',  'On-call alert routing configured for Team Workspaces surface',     'Omar Farouk',   'done',        '2026-04-25', 'Routes to Omar, escalates to Marcus after 15min'),
(9,  'atlas-ga-2026-04-29', 'comms',       'Customer-facing release notes approved',                           'Jamie Okafor',  'in_progress', '2026-04-28', 'Awaiting final review after defect resolution'),
(10, 'atlas-ga-2026-04-29', 'comms',       'Internal #atlas-eng and #northlight-all announcement drafted',      'Priya Chen',    'done',        '2026-04-28', 'Scheduled to post at cutover start'),
(11, 'atlas-ga-2026-04-29', 'support',     'Support macros + FAQ published for Team Workspaces',               'Fatima Noor',   'done',        '2026-04-24', 'ATLAS-134, published to internal wiki'),
(12, 'atlas-ga-2026-04-29', 'support',     '15-minute Support sync: what shipped, known issues, escalation path','Jamie Okafor', 'not_started', '2026-04-28', 'Scheduled for morning of 4/28'),
(13, 'atlas-ga-2026-04-29', 'rollback',    'Rollback runbook written and reviewed',                            'Omar Farouk',   'done',        '2026-04-25', 'See rollback_steps table, Lecture 3'),
(14, 'atlas-ga-2026-04-29', 'rollback',    'Rollback rehearsed against staging',                               'Omar Farouk',   'not_started', '2026-04-28', 'Scheduled dry-run before go/no-go');
```

Query what's actually blocking readiness, sorted so the highest-stakes gaps surface first:

```sql
SELECT category, item, owner, status, due_date
FROM release_checklist
WHERE release_name = 'atlas-ga-2026-04-29'
  AND status IN ('blocked', 'not_started', 'in_progress')
ORDER BY
    CASE status WHEN 'blocked' THEN 1 WHEN 'in_progress' THEN 2 ELSE 3 END,
    due_date;
```

Six of fourteen items aren't `done` yet, and one is `blocked` outright, four days before the target date. That's not automatically a reason to slip the date — it's the exact input the go/no-go decision (§4) needs to reason about.

## 3. The known defect list — the other half of the go/no-go picture

The DoD's `not_met` sign-offs (Lecture 1) named symptoms without naming the actual bugs behind them. Here's Atlas's real defect list as of go/no-go week — the artifact Yuki Tanaka and Marcus Diallo actually argue over:

```sql
INSERT INTO known_defects (defect_id, release_name, issue_key, summary, severity, status, discovered_on, assignee, workaround, customer_facing) VALUES
(1, 'atlas-ga-2026-04-29', 'ATLAS-131', 'Webhook signature verification fails intermittently under sustained high load (>2.1x normal peak)', 'major',  'open', '2026-04-22', 'Marcus Diallo', 'None yet — retried requests eventually succeed, but P99 latency spikes past customer-visible thresholds', TRUE),
(2, 'atlas-ga-2026-04-29', 'ATLAS-133', 'SLA dashboard shows stale data for up to 90 seconds after a Platform webhook timeout',        'major',  'open', '2026-04-23', 'Fatima Noor',   'Manual dashboard refresh clears it; no fix required before GA, but customers could see a false "all clear"', TRUE),
(3, 'atlas-ga-2026-04-29', 'ATLAS-135', 'Export CSV button label is misaligned by 2px on Safari 17',                                    'minor',  'open', '2026-04-24', 'Nadia Petrova', 'Purely cosmetic, no functional impact', TRUE),
(4, 'atlas-ga-2026-04-29', 'ATLAS-118', 'Memory leak in websocket manager under long-lived connections',                                'major',  'fixed','2026-03-05', 'Chris Okoye',   NULL, FALSE),
(5, 'atlas-ga-2026-04-29', 'ATLAS-121', 'Intermittent 502s from the sandbox during Sprint 6 integration testing',                       'critical','fixed','2026-03-19', 'Wei Zhang',    NULL, FALSE);
```

Query the defects that actually matter for this week's decision — the open ones a customer could notice:

```sql
SELECT issue_key, summary, severity, assignee, workaround
FROM known_defects
WHERE release_name = 'atlas-ga-2026-04-29' AND status = 'open' AND customer_facing = TRUE
ORDER BY CASE severity WHEN 'blocker' THEN 1 WHEN 'critical' THEN 2 WHEN 'major' THEN 3 ELSE 4 END;
```

Three rows: one major with no real fix yet (ATLAS-131 — the same root cause behind three failed DoD lines), one major with a workaround (ATLAS-133), one cosmetic minor (ATLAS-135). None of the three is a `blocker`-severity defect that would make this an easy no-go — and none is a clean `minor`-only list that would make this an easy go. That's exactly the harder, more realistic shape of go/no-go decision Challenge 1 works through.

## 4. Staged rollout vs. big-bang: choosing the cutover strategy

A **big-bang release** flips the switch for 100% of users at once. A **staged rollout** exposes the change to a small slice first, then widens it if nothing goes wrong.

**Common staged patterns:**

- **Canary release** — a small, low-risk slice of real traffic (often internal users or a single low-stakes customer) gets the new version first, watched closely, before anyone else.
- **Percentage-based rollout** — a feature flag exposes the change to 10%, then 50%, then 100% of users, with a pause and a health check at each stage. This is Atlas's plan (flag `team_workspaces_ga`, stages configured in DoD line 2).
- **Blue-green deployment** — two full production environments exist; traffic cuts over from the old ("blue") to the new ("green") environment, with the old one kept warm for an instant revert.
- **Feature-flagged dark launch** — the code ships to production but stays off for everyone, then gets turned on independently of any deploy — decoupling "shipped" from "live," which is exactly what makes rollback fast (Lecture 3).

**Choose staged when:**

- The failure mode is severe or hard to predict (Atlas's signature-verification path, still failing load tests four days out, is a textbook case).
- You can meaningfully detect a problem in the small slice before it reaches everyone — you have real monitoring, not just hope.
- The change is reversible per-user or per-cohort (a flag can be flipped back for the 10% without touching the 90% who never saw it).

**Big-bang is defensible when:**

- The change is small, low-risk, and well-tested, and staging it adds coordination cost without adding real safety.
- The change *can't* be partially applied — some database migrations, some breaking API version cutovers, some infrastructure changes are inherently all-or-nothing.
- The team has genuinely high confidence from history: this exact kind of change has shipped cleanly many times before.

Atlas is a clear staged case: real, currently-unresolved risk (ATLAS-131), a real dependency on another team's infrastructure (Sofia Reyes's Platform team), and a flag already built for exactly this purpose. Shipping 100% big-bang on April 29 with a known-flaky load path would be choosing the harder, riskier option when a safer one already exists and costs almost nothing extra.

## 5. The go/no-go decision

A **go/no-go decision** is a specific, scheduled meeting (or synchronous check, for a smaller release) where a named group of people look at the actual current state — DoD status, checklist status, open defects, monitoring readiness — and make an explicit, recorded call. It is not a vibe check, and it is not one person's private judgment call dressed up as a meeting.

**Who's in the room, and what they own:**

| Role | Owns the answer to |
|---|---|
| Tech Lead (Marcus Diallo) | Is the code, in fact, ready — DoD categories 1–2? |
| QA Lead (Yuki Tanaka) | Do the test results and open defects support a safe release? |
| SRE / on-call (Omar Farouk) | Can we operate this safely — monitoring, rollback, on-call coverage? |
| Dependency owner (Sofia Reyes, Platform) | Is the infrastructure this release depends on actually stable? |
| Product Owner (Elena Cruz) | Does the shipped scope match what was promised? |
| Sponsor (Priya Chen) | Given everything above, does this still meet the business need on this date, or does the date move? |

**The decision matrix.** A go/no-go call has exactly three honest outcomes, and "we'll figure it out as we go" is not one of them:

- **Go** — release proceeds as planned, on schedule.
- **No-go** — release does not proceed; a new date and the specific blockers to clear before then are named explicitly.
- **Conditional go** — release proceeds, but *narrower* than originally planned, with the condition stated precisely (e.g., "go, but hold the rollout at 10% until the signature-verification success rate holds above 99% for 24 hours" or "go, minus the affected feature, fast-followed once fixed").

Every decision gets recorded, not just remembered:

```sql
INSERT INTO release_signoffs (signoff_id, release_name, role, name, decision, condition_notes, decided_at) VALUES
(1, 'atlas-ga-2026-04-29', 'Tech Lead',         'Marcus Diallo', 'pending', NULL, NULL),
(2, 'atlas-ga-2026-04-29', 'QA Lead',           'Yuki Tanaka',   'pending', NULL, NULL),
(3, 'atlas-ga-2026-04-29', 'SRE / On-call',     'Omar Farouk',   'pending', NULL, NULL),
(4, 'atlas-ga-2026-04-29', 'Platform Team Lead','Sofia Reyes',   'pending', NULL, NULL),
(5, 'atlas-ga-2026-04-29', 'Product Owner',     'Elena Cruz',    'pending', NULL, NULL),
(6, 'atlas-ga-2026-04-29', 'Sponsor',           'Priya Chen',    'pending', NULL, NULL);
```

A `pending` row is not a `go` by default — the whole design point of this table is that a release with any `pending` signoff has, by definition, not been decided yet. Challenge 1 walks through exactly how Atlas resolves this table with real open defects still on the board.

## 6. Release communications

Release comms use the same audience-message-channel-cadence discipline from Week 7, applied to one specific event instead of an ongoing project:

- **Internal, before:** engineering and support know the exact cutover window and what to watch (the #atlas-eng announcement, checklist item 10).
- **Internal, during:** a live channel or thread where anyone can post what they're seeing in real time — this is the raw material Lecture 3's timeline reconstruction depends on if anything goes wrong.
- **External, at launch:** customer-facing release notes (checklist item 9) — accurate, not aspirational; a release note that oversells a feature with known rough edges creates the exact kind of support burden Jamie Okafor is trying to avoid going into the QBRs.
- **External, after:** if something did go wrong publicly, a short, honest customer-facing update — covered in Lecture 3's postmortem material.

The golden rule, carried over directly from Week 7: **comms during a release should never bury a slip.** If the rollout pauses at 10% because of a monitoring signal, the internal channel says that plainly and immediately — not "still rolling out," which is technically true and functionally a lie by omission.

## 7. Check yourself

- What's the difference in *scope* between the DoD (Lecture 1) and the release readiness checklist? Give one item that belongs only in the checklist.
- Why does Atlas's open defect list (§3) make go/no-go harder than either "all clean" or "one blocker" would?
- Name three staged-rollout patterns and one condition under which a big-bang release is still the right call.
- Who are the six roles in Atlas's go/no-go, and what does each one specifically own the answer to?
- What are the three honest outcomes of a go/no-go decision? Why is "let's just watch it closely" not a fourth valid outcome?
- Why does a `pending` row in `release_signoffs` matter as much as a `no_go` row — what does it prevent?
- What's the release-comms version of Week 7's "never bury a slip" rule?

If those are automatic, Lecture 3 covers what happens if the go/no-go says go and something breaks anyway — the rollback plan, and the post-release review that follows either way.

## Further reading

- **Martin Fowler — "BlueGreenDeployment":** <https://martinfowler.com/bliki/BlueGreenDeployment.html>
- **Martin Fowler — "CanaryRelease":** <https://martinfowler.com/bliki/CanaryRelease.html>
- **Atlassian — "Feature flags":** <https://www.atlassian.com/microservices/microservices-architecture/feature-flags>
- **Google SRE Book — "Release Engineering":** <https://sre.google/sre-book/release-engineering/>
