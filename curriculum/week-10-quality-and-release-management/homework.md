# Week 10 — Homework

Five problems, ~4 hours total, spread across the week. These reinforce the lectures with a mix of applied writing, structured analysis, and one interview-style reflection. Commit each.

Problems 1–3 reference Project Atlas and Northlight Software; see [Lecture 1](./lecture-notes/01-definition-of-done-and-gates.md) for the current cast, including this week's two new names (Yuki Tanaka, QA Lead, and Omar Farouk, SRE).

---

## Problem 1 — Find a real DoD gap in something you've shipped (45 min)

In `real-dod-gap.md`, think of something you've actually built and shipped — a school project, a piece of open-source code, a small app, a group assignment turned in as "done." Write the definition of done you *actually* used at the time (even if it was never written down — reconstruct it honestly from what you actually checked before calling it finished). Then write the DoD you *should* have used, spanning all four categories from Lecture 1, §2. Name the specific gap between the two, and describe one real consequence that gap caused (a bug that shipped, a confused user, a late-discovered missing doc) — or, if nothing went wrong, explain honestly whether that was skill or luck.

---

## Problem 2 — Rewrite a real "we'll figure it out as we go" decision (30 min)

In `decision-rewrite.md` (200–300 words): recall a real situation — a group project, a work task, an event you helped run — where a decision that should have had three clear outcomes (go / no-go / conditional-go, in spirit if not in name) instead got resolved by someone saying some version of "let's just see how it goes." What actually happened as a result? Rewrite that moment using Lecture 2, §5's decision matrix — what would `go`, `no_go`, and a *precise* `conditional_go` have actually looked like in that specific situation?

---

## Problem 3 — Design a rollback for something with no feature flag (45 min)

Pick something you could imagine "releasing" that has **no equivalent of a feature flag** — a physical event, a policy change, a new process at work or school, a recipe you're serving to guests for the first time. In `no-flag-rollback.md`, write a real rollback plan for it using Lecture 3, §1–2's structure: at least 2 observable trigger conditions, 3–4 ordered steps with owners (even if the owner is just "me"), and a verification step. The goal is proving the discipline generalizes past software — a wedding caterer, a store manager rolling out a new checkout process, and a release engineer are all solving the same underlying problem.

---

## Problem 4 — Interview someone about a release that went wrong, or analyze one from a distance (60 min)

Pick **one**:

- **(A)** Ask someone with real release or on-call experience: "Tell me about a release that went wrong, and what changed in how your team ships afterward." Write up their answer in `interview.md`, and specifically note whether their team's response afterward was blameless (Lecture 3, §4) or not — and if not, what effect that had on whether people reported problems honestly the *next* time.
- **(B)** If you can't find someone to interview, find a **public postmortem** from a well-known technology company (many publish these after major outages) and analyze it through Lecture 3, §4's four-step lens in `postmortem-analysis.md` (250–400 words): does it reconstruct a real timeline, ask "why" toward a systemic cause, name genuine contributing factors, and produce specific owned action items — or does it, despite good intentions, still slip into blaming a person or team? Cite your source.

---

## Problem 5 — Write the go/no-go message for something absurdly small (30 min)

The go/no-go discipline scales down the same way Week 7's communication planning did. In `tiny-go-no-go.md`, pick something small and concrete you're actually planning (a trip, a small event, submitting something with a real deadline) and write a genuine go/no-go: 2–3 real readiness criteria specific to that thing, and a final decision (go / no-go / conditional-go) with your actual reasoning. The goal is proving that "am I actually ready, or do I just feel ready" is a useful question at any scale, not just for company releases.

---

## Time budget

| Problem | Time |
|--------:|-----:|
| 1 | 45 min |
| 2 | 30 min |
| 3 | 45 min |
| 4 | 60 min |
| 5 | 30 min |
| **Total** | **~3.5 h** |

After homework, take the [quiz](./quiz.md) and ship the [mini-project](./mini-project/README.md) if you haven't already.
