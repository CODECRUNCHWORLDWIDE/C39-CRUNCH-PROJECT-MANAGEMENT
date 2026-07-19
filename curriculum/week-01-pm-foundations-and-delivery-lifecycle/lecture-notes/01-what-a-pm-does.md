# Lecture 1 — What a Project Manager Actually Does

> **Duration:** ~2 hours. **Outcome:** You can name the difference between a project, a product, and operations; place the PM correctly among the sponsor, product owner, tech lead, and delivery team; and describe the PM's real day-to-day job in one honest paragraph instead of a job-title cliché.

Ask ten people what a project manager does and you'll get ten answers, most of them wrong: "makes the schedule," "runs meetings," "assigns tasks," "chases people for status." Those are things a *bad* PM does. A good PM does something much narrower and much harder: **removes ambiguity and removes blockers, continuously, until the thing ships.** Everything else in this course is technique in service of that one sentence.

## 1. Meet the case study: Project Atlas

We'll use one running scenario all week so the concepts have somewhere to live.

**Northlight Software** is a 40-person B2B analytics company. Its product, **Northlight Insights**, lets marketing teams build dashboards from ad-spend data. Customers have been asking for a way to share a dashboard with teammates and comment on it together — right now every account is single-user. Leadership approves an initiative to build this: **Project Atlas**, a "Team Workspaces" feature.

The people involved:

- **Priya Shah — VP of Product (the sponsor).** Owns the budget and the "why." She wants Atlas because three enterprise renewals are at risk without team collaboration, and a competitor shipped something similar last quarter. She does not want to run the project day to day — she wants it to *succeed* and to know quickly if it's at risk.
- **Elena Cruz — Product Owner.** Owns the "what." She talks to customers, prioritizes what Atlas needs to do first, and makes the call on trade-offs inside scope (e.g., "comments before sharing, or sharing before comments?").
- **Marcus Webb — Tech Lead.** Owns the "how." He and his four engineers design the system, estimate the work, and are the ones who actually know whether a date is realistic.
- **You — Project Manager.** Own the "when, and what's in the way." You don't write the requirements (Elena does) and you don't design the system (Marcus does). You make sure the requirements, the design, the schedule, and the stakeholders stay connected to each other and to reality — and you're the first to know, and say out loud, when they don't.

Keep these five people in mind. Every lecture, exercise, and challenge this week refers back to them.

## 2. Project vs. product vs. operations

These three words get used interchangeably in casual conversation and that's exactly the problem — they require different management approaches, and confusing them is one of the most common reasons technology initiatives quietly fail.

| | Project | Product | Operations |
|---|---|---|---|
| **Definition** | A temporary effort with a defined start, end, and outcome | An ongoing thing you build, ship, and evolve indefinitely | The repeatable work that keeps the business running |
| **Ends when** | The outcome is delivered and accepted | It doesn't — the product lives until it's sunset | It doesn't — it's continuous by design |
| **Success looks like** | Hit the defined scope/time/cost/quality target | Sustained user/business value over time | Consistent, predictable, low-variance execution |
| **Example at Northlight** | Project Atlas — ship Team Workspaces by the target date | Northlight Insights — the product as a whole, forever | Nightly data-pipeline runs, customer support tickets, on-call |
| **Who typically owns it** | A project manager (or a PM-hatted lead) | A product manager | Team leads / SRE / support managers |

Project Atlas is a **project** — it has a specific outcome (team workspaces exist and work), a target window, and it *ends*: once shipped and adopted, the "Atlas" effort is closed even though the product, Northlight Insights, keeps evolving forever. The distinction matters because you manage a project toward a finish line and manage a product toward a trajectory — different cadences, different success measures, different mindsets.

**The trap:** many technology teams run *product* work with *project* thinking (treating every feature as a mini-project with its own charter and Gantt chart — exhausting and unnecessary) or run *project* work with *product* thinking (letting Atlas's scope drift forever because "we'll just keep iterating," which is how six-month projects become eighteen-month projects with nobody able to say when it's "done"). Naming which one you're actually doing is the first decision, and it's a PM decision.

**Operations** is the third category and the easiest to misclassify: "make the CI pipeline faster" sounds like a project (it has a goal!) but if it's really "we continuously tune CI as part of normal engineering work," it's ops, not a project — don't write it a charter, just do it. A good rule of thumb: **if it has no real end date and no real "done," it's product or ops, not a project.**

## 3. What the PM actually does, day to day

Strip away the job-title mythology and a PM's real work on a technology team is four things, repeated in a loop:

### a) Frame the work

Turn a vague ask ("customers want collaboration") into a scoped, sequenced, agreed plan. This is Lecture 3's charter — but framing isn't a one-time document, it's a habit. Every time scope gets fuzzy mid-project ("wait, does Atlas include real-time cursors or not?"), the PM's job is to re-frame it: get the right people in a room (or a thread), force a decision, and write it down.

### b) Unblock the team

This is the highest-leverage, least-glamorous part of the job. Marcus's team is blocked because the design needs a decision from Elena about whether comments are per-workspace or per-dashboard. A PM who's good at this notices the blocker *before* it costs three days of idle engineering time, chases the decision, and gets an answer — not by making the decision themselves (that's not their call) but by making sure the decision-maker knows it's needed and knows the deadline for making it.

### c) Communicate status honestly

Priya, the sponsor, cannot see the work happening — she sees what the PM tells her. If the PM says "on track" every week until the week before the deadline, when it becomes "actually, we're three weeks late," that's not a scheduling failure, it's a **trust failure**, and it's the single fastest way to lose the confidence of a sponsor. Good status communication surfaces bad news *early and small*, not late and large — "we're tracking two days behind on the sharing API, here's why, here's the plan" beats silence every time.

### d) Manage risk before it's a crisis

A PM who's only reacting to problems that have already happened is running the project one week too late. Part of the job is asking "what could go wrong here, and what would we do about it" *before* it goes wrong — this becomes the risk register in Week 6, but the habit of thinking ahead starts now.

Notice what's **not** on this list: writing code, designing the database schema, deciding what features ship, or personally deciding who does what task each day. Those belong to the tech lead, the architecture, the product owner, and the individual engineers respectively. A PM who starts doing those jobs is either duplicating someone else's work (annoying) or overriding it (worse).

## 4. The roles around the PM, and where the boundaries are

Confusion about "who decides what" is the single most common source of friction on technology teams. Here's the boundary, using Northlight's people:

- **Sponsor (Priya)** — owns *why* the project exists and *whether it's worth the money*. Approves the charter, unblocks organizational obstacles (budget, headcount, cross-team priority fights) that are above the PM's authority to resolve alone, and is the final escalation point. The PM keeps her informed; she doesn't run the project day to day.
- **Product Owner (Elena)** — owns *what* gets built and in what order. Writes/approves requirements, prioritizes the backlog, and makes scope trade-off calls inside the approved charter. The PM does not override Elena's prioritization calls — but the PM *does* flag when a prioritization choice threatens the timeline or budget Priya approved, so it gets decided with eyes open.
- **Tech Lead (Marcus)** — owns *how* it gets built and *whether an estimate is realistic*. Designs the system, breaks down technical work, and is the authority on "can we actually do this by that date." The PM does not set technical direction or override an engineering estimate — but the PM does push back when an estimate seems to conflict with what was promised elsewhere, so that gets resolved instead of silently absorbed.
- **Delivery team (Marcus's four engineers)** — owns *doing* the work and surfacing blockers as they hit them. The PM does not assign individual tasks in a healthy Agile team (Week 2 covers why) — the team pulls work; the PM makes sure the *board* and the *plan* make sense so pulling work is possible.
- **Project Manager (you)** — owns *the plan staying true, and everyone staying aligned to it or to an explicit, agreed change to it.* You have very little unilateral authority over scope, technical approach, or requirements — and that's by design. Your authority is over the *process*: is scope decided and written down, is the schedule realistic and current, is everyone talking to each other, is risk visible.

A one-line test that resolves most "wait, whose call is this?" arguments: **ask what kind of decision it is.** A "why are we doing this at all / is it worth it" decision is the sponsor's. A "what should it do" decision is the product owner's. A "how do we build it / can we hit this date" decision is the tech lead's. A "is everyone aligned and is this still on track" question is the PM's — and the PM's job is often to *notice* that a decision hasn't been made and needs to be, then get it in front of the right person, not to make it themselves.

## 5. A day in the life, briefly

To make this concrete: on a normal Tuesday during Project Atlas, the PM might spend the morning reviewing the sprint board and noticing that a story about workspace permissions has been "in progress" for six days with no comments — a possible blocker hiding in plain sight. A quick message to the engineer surfaces that they're stuck waiting on a design decision from Elena. The PM pings Elena, gets the decision within the hour, and the story unblocks. At 1pm there's a 15-minute sync with Priya where the PM reports: on track, one risk to watch (a third-party API Atlas depends on has a flaky sandbox environment, logged in the risk register). In the afternoon the PM updates the delivery plan because Marcus flagged that the sharing feature will take longer than estimated, and drafts a short note for Elena and Priya laying out two options: slip the date two weeks, or cut real-time presence indicators from this release. That note — not a Gantt chart, not a status meeting — is the actual work of project management: **turning a technical reality into a clear decision for the people who own that decision.**

## 6. Check yourself

- In one sentence, what's the difference between a project and a product? Where does Project Atlas fall, and why?
- Name one thing a PM does that is *not* on this lecture's four-item list, and explain why it actually belongs to another role.
- Marcus says a date is unrealistic. Whose decision is it to change the date, and what's the PM's job in that conversation?
- Why is "surfacing bad news early and small" better project management than "staying positive until it's undeniable"?
- Give an example of work that looks like a project but is actually operations, and explain the tell.

If those are automatic, Lecture 2 walks Project Atlas through the full delivery lifecycle — the phases the PM's four core activities happen inside of.

## Further reading

- **PMI — "What is Project Management?":** <https://www.pmi.org/about/what-is-project-management>
- **Atlassian — "What is a project manager?":** <https://www.atlassian.com/agile/project-management>
- **Martin Fowler — "Product vs Project":** <https://martinfowler.com/articles/products-over-projects.html>
