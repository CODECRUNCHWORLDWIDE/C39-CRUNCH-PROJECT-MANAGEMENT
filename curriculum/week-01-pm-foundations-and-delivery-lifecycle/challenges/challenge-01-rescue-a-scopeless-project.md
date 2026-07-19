# Challenge 1 — Rescue a Scopeless Project

**Time:** ~90 minutes. **Difficulty:** Medium-hard. **No single right answer.**

## The scenario

You've just been brought in as PM on a project that's been running for **six weeks with no charter, no written scope, and no defined success criteria.** This happens more often in the real world than anyone likes to admit: someone said "let's build X," a couple of engineers started, and momentum carried it forward without anyone ever stopping to write down what "done" means.

Here's what you know, gathered from a week of conversations and reading old Slack threads:

- The project is internally called **"Reports Revamp."** It started because a senior engineer, Yusuf, felt the existing reporting export in Northlight Insights was "clunky" and started rebuilding it in his spare time. Two other engineers joined in over the following weeks because it seemed useful.
- There is no written scope. In conversation, Yusuf describes the goal as "make reporting not suck" — when pressed for specifics, he mentions faster CSV exports, a new PDF export option, and "maybe scheduled email reports, that'd be cool."
- Priya (VP Product) knows the project exists — she approved the engineers' time informally — but has never seen a plan, a date, or a scope document. When you ask her what she expects, she says "I trust the team, just make sure it's useful and doesn't take forever."
- Elena (Product Owner) has not been closely involved. She mentions customers have asked for faster exports (a real, validated pain point) but has never heard of the "scheduled email reports" idea and isn't sure it's a priority.
- The engineers have working code for faster CSV exports (roughly done) and partial, unfinished work on PDF export. Nothing exists yet for scheduled email reports beyond a design doc Yusuf wrote himself.
- Six weeks of three engineers' time have been spent. Nobody can currently answer "when will this be done" or "what does done mean" with a straight face.

## Your task

Write `rescue-plan.md` containing:

1. **Diagnosis (1 paragraph).** Using Lecture 2's language, name what phase this project actually skipped, and explain — specifically, using details from this scenario — what that skip cost so far (be concrete: what's the actual damage, not just "it's disorganized").

2. **A retroactive charter.** Yes, retroactive — write the charter this project *should* have had, as if you're writing it now, six weeks late, incorporating what's actually been built so it isn't wasted. Include: objective, success criteria, in/out of scope, constraints, and decision authority (per Lecture 3's anatomy). You will have to make and **state** judgment calls — e.g., is "scheduled email reports" in or out of this release? Justify your call using what you know about Elena's and Priya's actual priorities, not just what's convenient given the code that already exists.

3. **A recommendation for the existing PDF-export and scheduled-email work.** For each of the two unfinished/uncertain pieces, recommend: keep it in scope, cut it, or explicitly defer it to a later release — and defend the call in 2–3 sentences each, using the triple constraint (Lecture 3, §3) and Elena's/Priya's stated priorities as your evidence.

4. **One process change** you'd put in place going forward so this doesn't happen again on the *next* project at Northlight — not "write better charters" (too vague) but something concrete and specific (e.g., a rule about when work can start, a required sign-off, a lightweight check-in cadence).

## Constraints

- You cannot un-spend the six weeks already spent — your rescue plan has to work *with* the existing partial work, not pretend it doesn't exist.
- Don't invent facts not given above (e.g., don't assume a specific deadline exists — none was given; if your plan needs one, say what you'd ask Priya for and why).
- Every judgment call must be stated explicitly, the way Lecture 3 and this week's exercises modeled. An unstated assumption is an incomplete answer even if your plan is otherwise reasonable.

## How success is judged

| Signal | Weak answer | Strong answer |
|--------|-------------|----------------|
| Diagnosis | "It was disorganized" | Names the skipped phase and traces a specific, concrete cost to that skip |
| Charter quality | Vague objective, no real numbers in success criteria | Passes Lecture 3's success-criteria test; scope is concrete and has explicit exclusions |
| Judgment on existing work | Keeps everything because it exists, or cuts everything to be safe | Each of the three pieces (CSV, PDF, scheduled email) gets a distinct, justified call |
| Process change | Generic ("communicate better") | Specific, enforceable, and clearly would have prevented this exact failure |

## Submission

Commit `rescue-plan.md` to your portfolio under `c39-week-01/challenge-01/`.
