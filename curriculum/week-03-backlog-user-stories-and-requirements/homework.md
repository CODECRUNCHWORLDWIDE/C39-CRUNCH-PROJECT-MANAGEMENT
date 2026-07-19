# Week 3 — Homework

A practice set that ties the week together, spread across the week rather than crammed in one sitting. None of this is graded, but the mini-project assumes the reps below are already comfortable — don't skip to Saturday.

**Estimated total time:** ~4 hours, spread Monday–Friday.

## 1. Story-spotting in the wild (45 min)

Find a real, public backlog you can browse — an open-source project's GitHub Issues (label search for `type: story` or similar), a public Jira/Trello board, or even a product's public roadmap comments. Find **five real examples**:

- Two that are genuinely well-formed user stories (or close to it).
- Two that are actually bugs or technical tasks mislabeled as stories.
- One that's a compound story bundling multiple capabilities.

For each, write one sentence identifying which category it's in and why. This is the single fastest way to make the anti-patterns from Lecture 1 §5 pattern-match instantly instead of needing a checklist every time.

## 2. Rewrite your own recent request (30 min)

Think of a real thing you've asked someone for recently — a feature request to an app you use, a favor from a friend, a change you asked a coworker for. Write it as a proper role–goal–benefit user story. If you can't write a real benefit, that's telling you something true about the original ask — write one sentence about what it tells you.

## 3. Slice a feature from an app you use daily (45 min)

Pick one feature from an app on your phone right now (e.g., "search," "notifications settings," "profile editing") and imagine you're the PM asked to slice it as an epic into a first release's worth of stories. Write 4–6 thin, vertical stories using at least two slicing patterns from Lecture 2 §4. Don't overthink the app's actual internals — the point is practicing the *slicing move*, not reverse-engineering someone else's codebase.

## 4. Given/When/Then drill (30 min)

Pick any login form you've used. Write acceptance criteria for "user logs in with email and password" — one happy path, and at least three edge cases (wrong password, account doesn't exist, too many failed attempts, empty fields — pick any three). This is deliberately a familiar, well-understood flow so you can focus entirely on the *format* of Given/When/Then rather than the underlying requirements.

## 5. MoSCoW your own week (30 min)

Take your actual personal to-do list for this coming week (real tasks, not hypothetical ones) and sort it into MoSCoW buckets. Notice how many things you'd normally treat as equally urgent turn out to have very different real priority once you're forced to bucket them — that discomfort is exactly what a stakeholder feels the first time you make them do this to their backlog.

## 6. Read one real backlog-grooming disaster story (30 min)

Search for a postmortem, blog post, or conference talk about a project that failed (or nearly failed) specifically because of poor requirements/backlog practices — vague stories, no acceptance criteria, scope creep through an ungroomed backlog, etc. Write a 3–4 sentence summary connecting what went wrong to a specific concept from this week's lectures (INVEST, the Three Cs, vertical slicing, Definition of Ready).

## 7. Optional stretch: interview someone about a real need (30–45 min)

If you can, do a 10-minute mock elicitation interview with a friend, classmate, or family member about something they actually wish worked better (an app, a household process, anything). Practice the open-question technique from Lecture 1 §4 — ask "walk me through what you do today," not "would you like feature X?" Write the raw notes and one story you'd draft from them.

---

Come to the [quiz](./quiz.md) after finishing the mini-project — the quiz assumes this week's full arc (elicitation → stories → slicing → acceptance criteria → prioritization) is fresh.
