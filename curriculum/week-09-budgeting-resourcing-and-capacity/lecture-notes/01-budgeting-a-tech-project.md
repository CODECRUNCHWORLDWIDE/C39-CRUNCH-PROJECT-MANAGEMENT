# Lecture 1 — Budgeting a Technology Project

> **Duration:** ~2 hours. **Outcome:** You can name the real cost components of a technology project, build a labor-dominated budget from rates and planned hours, size a defensible contingency reserve, and explain why a budget is a live signal you check weekly, not an annual guess you file away.

Ask a new PM "what's the project's budget?" and most will say a single number — "$280,000" — as if it were a fact discovered once, in a planning meeting, and then true forever. It isn't. A budget is a **forecast** built from assumptions (rates, hours, scope, timeline) that were your best guess on day one, and every week that passes either confirms those assumptions or breaks one. This lecture builds Project Atlas's actual budget from the ground up, so the number you land on isn't a guess you defend by authority — it's a sum you can defend line by line.

## 1. What actually costs money on a technology project

Ask a non-technical stakeholder to guess a software project's biggest cost and they'll often say "the software" or "the servers." They're almost always wrong. On the overwhelming majority of technology projects — and Atlas is typical — **labor is the dominant cost**, usually 70–90% of the total. People's time, not tools, is what you're spending.

The four categories worth tracking separately:

| Category | What it covers | Typical share |
|---|---|---|
| **Labor** | Salaried/contractor time actually spent on the project — the biggest line by far | 70–90% |
| **Tooling** | SaaS subscriptions, infrastructure, licenses directly attributable to the project | 2–8% |
| **Vendor / third-party** | Contracts with outside parties — a paid API, a consultancy, a specialist contractor engagement priced as a fixed fee rather than hourly labor | 0–15%, project-dependent |
| **Contingency** | A reserve for the risks you already know about (from your Week 6 risk register) and the ones you don't | 10–20% of the above |

Atlas's shape is typical: a small core team (labor), a handful of SaaS tools plus CI/infra (tooling), a paid third-party real-time API — the same one whose flaky sandbox you logged as a risk in Week 6 (vendor) — and a contingency reserve sized against exactly that kind of known-unknown.

**What this lecture deliberately does not cover:** capital expenditure, depreciation, overhead allocation (office space, benefits load beyond what's baked into the hourly rate), and tax treatment. Those are real and matter to Finance, but they're accounting-department concerns, not PM-level budget-building — the fully loaded hourly rate you'll use already has benefits and overhead folded in by Finance before it ever reaches you.

## 2. Building the labor line — rate × planned hours

Labor cost is arithmetic, but the arithmetic only works if you get two inputs right: **who**, at **what fully loaded rate**, for **how many hours**.

**The roster.** Atlas's core team, with fully loaded hourly rates (what it actually costs the company per hour of that person's time — salary, benefits, and overhead already folded in by Finance, not just take-home pay):

| Person | Role | Type | Rate ($/hr) | Weekly capacity |
|---|---|---|---:|---:|
| Marcus Webb | Tech Lead | employee | 145 | 40h |
| Priya N. | Senior Engineer | employee | 120 | 40h |
| Sam O. | Engineer | employee | 95 | 40h |
| Dana K. | Engineer | employee | 95 | 40h |
| Wes T. | Engineer | employee | 90 | 40h |
| Yuki Tanaka | Frontend Contractor | contractor | 110 | 30h (part-time) |
| Elena Cruz | Product Owner | employee | 100 | 40h, partial on Atlas |
| Sofia Reyes | Platform Team Lead | employee | 150 | 40h, partial on Atlas |
| PM (You) | Project Manager | employee | 110 | 40h |

Notice three things already, before a single query runs. First, **not everyone is full-time on Atlas** — Elena and Sofia both split their week across other work, which is exactly why Week 9 needs an *allocation* model, not just a flat "5 people × 16 weeks." Second, **a contractor's rate isn't automatically comparable to an employee's** — Yuki's $110/hr looks close to Priya's $120, but Yuki's rate has to cover things an employer absorbs for employees (their own overhead, no PTO paid by you, no benefits from you); comparing contractor and employee rates apples-to-apples requires knowing what's already folded in. Third, **weekly capacity isn't uniformly 40 hours** — Yuki's contract caps at 30h/week. Any capacity math that assumes everyone has the same ceiling will be wrong for exactly the people it matters most to get right.

**Planned hours.** For the labor budget line, you plan hours at the *project* level, not the individual level yet (that granular allocation is Lecture 3's job). Atlas is budgeted for roughly:

- Core delivery team (Marcus, Priya, Sam, Dana, Wes, Yuki): **1,300 planned hours** over the 16-week project → **$190,000** planned labor cost, at each person's blended rate.
- PM and partial Product time (You + Elena's Atlas-attributable hours): **~290 planned hours** → **$30,000**.

**Labor budget total: $220,000.** That number came from a roster and a planned-hours estimate, not a vibe — which means when it's wrong (and it will be, a little), you can find out *which* input was wrong instead of just knowing the total missed.

## 3. Tooling and vendor — the smaller but sharper-edged lines

**Tooling** ($8,000 planned) is the easiest line: SaaS subscriptions, CI minutes, a design-tooling seat, an added monitoring tool — recurring, predictable, low-drama. It rarely blows up a budget on its own, but it's worth tracking separately from labor because a tooling overrun is a completely different kind of problem (a subscription you forgot to cancel) than a labor overrun (the work is taking longer than planned) — mixing them into one "misc" bucket destroys your ability to diagnose which.

**Vendor** ($22,000 planned) is the sharper-edged line, and Atlas is a good teaching case for why. The contract for the third-party real-time API — the same vendor whose flaky sandbox environment you logged as risk `R-2` in Week 6 — was budgeted as a flat monthly support fee. In Week 6/7 terms, that risk was scored and had a mitigation plan. In budget terms, **a materializing risk usually shows up first as a vendor or labor cost spike**, not as a line item that says "risk occurred." When the sandbox instability got bad enough in November that the team needed emergency vendor support hours, the vendor line jumped from a flat $5,500/month to $9,800 that month — a fact the risk register predicted in kind (this was flagged as a risk with a cost-shaped consequence) but that only the budget can tell you in dollars. This is the concrete link between Week 6's risk work and this week's budget work: **a risk register without a budget can tell you something might hurt; a budget without a risk register can't tell you why it did.** Together they cover both halves.

## 4. Contingency — sizing a reserve you can defend

Contingency is not "a cushion because we're not confident." A defensible contingency reserve is sized against **specific, named risk exposure**, using the same probability × impact thinking from Week 6, not a flat "add 15% because that's what we always do" — though a flat percentage is a reasonable *starting point* when you don't yet have a scored risk register to size against.

Two legitimate ways to size it, in increasing order of rigor:

1. **Percentage-of-base method.** Take a percentage (commonly 10–20% for technology projects with moderate uncertainty) of the base budget (labor + tooling + vendor). Atlas: 12% of $250,000 (`$220,000 + $8,000 + $22,000`) = **$30,000**. Simple, defensible as an industry norm, but doesn't connect to *this project's specific* risks.
2. **Risk-based method.** Sum the expected cost impact of your top scored risks from the Week 6 register (probability × estimated dollar impact, for the risks that would actually cost money if they occurred), and set contingency at or slightly above that sum. This is more defensible in a room with a skeptical CFO because it answers "contingency for *what*, specifically" — but it requires the risk register to already have dollar-impact estimates, which not every team does.

Atlas uses the percentage method for simplicity this week (12% → $30,000), with a note in the budget documentation that the vendor risk alone (`R-2`) represents a meaningful share of that reserve's expected use — a hybrid in practice, which is normal.

**Total Atlas budget (BAC — Budget at Completion): $280,000.**

| Category | Planned amount |
|---|---:|
| Labor — core delivery team | $190,000 |
| Labor — PM & Product | $30,000 |
| Tooling | $8,000 |
| Vendor | $22,000 |
| Contingency | $30,000 |
| **Total (BAC)** | **$280,000** |

## 5. Contingency is a reserve, not a slush fund

The single most common way contingency fails is treating it as free money once the "real" budget looks tight. It isn't. Two disciplines keep it honest:

- **Contingency is drawn against, not blended in.** When the November vendor spike hit, the $4,300 overage against the vendor line's plan-to-date was recorded as a **contingency draw** — a separate, visible transaction against the contingency budget line — not silently absorbed into the vendor actuals as if nothing happened. Anyone looking at the budget can see both "vendor ran over" and "here's what covered it," instead of one number that quietly nets them together.
- **A draw needs the same trigger discipline as a risk response.** You don't draw contingency because a category "feels" tight — you draw it against a specific, named, dated event (here: "Nov vendor overage from emergency sandbox support hours, tied to risk R-2"). A contingency reserve with no draw log is indistinguishable from a made-up cushion nobody can account for later.

## 6. Reading burn as a live signal, not an annual guess

The habit this lecture is really trying to build: **a budget is only useful if you read it on a cadence, not once.** "We set a $280,000 budget in September" is a fact about the past. "We've spent $209,700 of $280,000 through the end of November, against a plan-to-date of $188,500" is a fact about *right now* — and it's the second one that tells you whether to act.

Two numbers do almost all the work, and you'll compute both precisely with SQL in Lecture 2:

- **Burn** — cumulative actual spend to date, broken down by category. "How much have we spent."
- **Variance** — plan-to-date minus actual-to-date. Negative means over budget; positive means under. "How much has that spend diverged from what we said it would be, at this exact point in the timeline" — critically, *not* compared to the full annual budget, which tells you almost nothing about whether October specifically ran hot.

Atlas's three months of closed actuals (Sep, Oct, Nov) already show a pattern worth naming before Lecture 2's queries make it precise: labor has been running a little over plan every single month, and the gap has been **widening**, not holding steady. A one-time overage is noise. A widening trend, month over month, is a signal — and it's exactly the kind of thing a stale annual number would hide until the money was already gone.

## 7. Check yourself

- Why is labor almost always the dominant cost on a technology project, and what does that imply about where your budgeting effort should go?
- What's the difference between a fully loaded hourly rate and a person's take-home pay, and why does the budget use the former?
- Name two legitimate ways to size a contingency reserve. Which is more defensible to a skeptical CFO, and why?
- Why should a contingency draw be logged as its own transaction instead of blended into the category that overran?
- What's the difference between "burn" and "variance," and why is variance measured against plan-*to-date*, not the full annual budget?
- Atlas's vendor spike in November traces back to a risk logged in Week 6. In your own words, what's the relationship between a risk register and a budget — what can each tell you that the other can't?

If those are automatic, Lecture 2 puts this exact budget into PostgreSQL and writes the queries that compute burn, variance, and a first forecast for real.

## Further reading

- **PMI — "Project Cost Management Overview":** <https://www.pmi.org/learning/library/cost-management-projects-basics-6228>
- **PMBOK-style contingency reserve guidance (Project Management Docs):** <https://www.projectmanagementdocs.com/template/project-planning/cost-management-plan/>
- **Atlassian — "How to create a project budget":** <https://www.atlassian.com/agile/project-management/project-budget>
