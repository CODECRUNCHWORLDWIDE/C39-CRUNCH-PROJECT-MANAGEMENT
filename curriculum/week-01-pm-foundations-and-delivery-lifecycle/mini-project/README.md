# Mini-Project — Write a Real Project Charter and Delivery Plan

> Pick a real build — your own idea, a feature for a project you already have, or one of the provided briefs — and write the charter and delivery plan that should exist before anyone writes a line of code. This is the week's capstone: everything from Lectures 1–3 lands in one concrete artifact.

**Estimated time:** 3 hours, best done Saturday after the exercises and challenges.

A charter you write for a fictional case study is a good rehearsal. A charter you write for something *real* — a side project, a feature you actually want to build, a tool your team actually needs — is proof you can do the job. That's the point of this mini-project: one real charter, held to the same bar as Project Atlas's.

---

## Step 1 — Choose your project

Pick **one**:

- **(A) Your own real idea.** A side project, an app feature, an internal tool at work — something you'd genuinely consider building. This is the strongest option if you have one; a charter for something you care about is easier to write honestly and more useful to keep.
- **(B) One of the provided briefs**, if you don't have a real project in mind:
  1. **"Study Buddy Matcher"** — a small web app that matches students in the same course into study groups based on availability and topic interest.
  2. **"Shift Swap"** — an internal tool for a retail chain letting hourly employees request and approve shift swaps without a manager manually coordinating every one.
  3. **"Community Tool Library"** — a neighborhood app where residents list tools they're willing to lend, and others can request and reserve them.

Whichever you pick, write **one sentence** at the top of your deliverable naming your choice and, if (A), a one-line description of what it is.

---

## Step 2 — Write the charter

Following Lecture 3's anatomy exactly, produce a **one-page charter** (`charter.md`) with these sections, in order:

1. **Objective** — why this matters, tied to a real consequence, not just a feature description (Lecture 3, §2).
2. **Success criteria** — at least 3, each passing the "would two reasonable people agree on this in 6 months" test (Lecture 3, §2 and Exercise 3).
3. **Definition of done** — distinct from success criteria; when is the *project* complete, independent of outcome (Lecture 3, §2).
4. **Scope — in and out** — at least 4 items in scope, at least 3 items explicitly out of scope for this release (Lecture 3, §2).
5. **Constraints** — at least 3 real constraints (a date, a budget/resource limit, a technical or compliance constraint) (Lecture 3, §2).
6. **Decision authority** — who decides what, even if it's just you wearing multiple hats — name the hats explicitly (sponsor/PO/tech lead/PM) and what each one owns, per Lecture 1's role boundaries.

Keep it to roughly one page (150–300 words is normal for a real charter — resist the urge to over-write; concision is a feature, not a shortcut).

---

## Step 3 — Choose and defend a delivery approach

In `delivery-approach.md`, 150–250 words:

- State your chosen approach: predictive, iterative, or Agile (Lecture 3, §4).
- Defend it using the specific facts of *your* project — how well-understood is the scope, how much do you expect to learn from real usage, what happens if requirements change mid-project.
- Name the lifecycle phase (Lecture 2) you're in right now (almost certainly initiation, having just written the charter) and briefly describe what planning would look like next for this project (you don't need a full backlog — 3–5 sentences on how you'd break the charter's scope into a first pass of work).

---

## Step 4 — Triple-constraint stress test

In the same `delivery-approach.md` file, add one short paragraph: pick **one** of your charter's constraints (date, budget, or scope) and describe what you would do if, three weeks in, it became clear that constraint and your stated scope could no longer both hold. Which lever would you pull, and why — using the triple constraint from Lecture 3, §3. This should be a real, specific answer, not "I'd figure it out."

---

## Deliverable

A directory in your portfolio `c39-week-01/mini-project/` containing:

1. `charter.md` — the one-page charter (Step 2).
2. `delivery-approach.md` — the approach defense, next-phase description, and stress test (Steps 3–4).

---

## Rubric

| Criterion | Weight | "Great" looks like |
|-----------|------:|--------------------|
| Objective clarity | 15% | Tied to a real consequence, not a restated feature list |
| Success criteria | 25% | All pass the measurability test; a number, a timeframe, a way to check |
| Scope precision | 20% | Concrete in-scope list; explicit, non-obvious out-of-scope exclusions |
| Constraints & authority | 15% | Real constraints stated as facts, not goals; decision authority named per role |
| Delivery approach defense | 15% | Cites specific project facts, not the textbook definition |
| Stress test | 10% | A real, defensible lever choice under the triple constraint, not a dodge |

---

## Stretch goals

- Write the charter for a **second**, deliberately different project (e.g., if your first was Agile-shaped, pick something regulatory or hardware-adjacent that should be predictive) and compare the two side by side in a short `comparison.md` — what changed in how you wrote the charter, and why.
- If you picked a real idea (option A), share the charter with one other person who knows nothing about the project and ask them to tell you, from the charter alone, what's in scope and what success looks like. If they get it wrong, your charter wasn't as clear as you thought — revise it and note what you changed.
- Draft the **decision-authority section** as an actual one-paragraph email you'd send to real stakeholders, in plain, non-jargon language.

---

## Why this matters

Every course week from here forward assumes a charter already exists — Week 2's Agile ceremonies, Week 3's backlog, Week 4's sprint plan all sit on top of the scope and success criteria you'd have written in initiation. Writing one real charter, held to this bar, is what turns "I understand what a charter is" into "I can produce one a sponsor would actually sign." Keep `charter.md` — you'll extend it directly in later weeks as you build out the backlog and delivery plan for the same project.

When done: push, then take the [quiz](../quiz.md) and start [Week 2 — Agile, Scrum & Kanban](../../week-02-agile-scrum-and-kanban/).
