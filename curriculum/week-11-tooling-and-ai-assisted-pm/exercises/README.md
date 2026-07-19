# Week 11 — Exercises

Three guided exercises. Unlike a pure-SQL week, these mix hands-on tool configuration (a real GitHub Projects board, real JQL against a Jira sandbox or this week's seed) with the SQL/pandas analysis muscle you've built since Week 8. Do them in order — Exercise 3 assumes the backlog discipline from Exercise 1 and 2's data fluency.

1. **[Exercise 1 — Automate a board](exercise-01-automate-a-board.md)** — stand up a real GitHub Projects board with custom fields and automations, then script its export into SQL. *(~1.5h)*
2. **[Exercise 2 — Write JQL queries](exercise-02-write-jql-queries.md)** — pull delivery data from a Jira project in JQL, then reproduce every query in SQL against `jira_issues`. *(~1.5h)*
3. **[Exercise 3 — AI, draft a backlog](exercise-03-ai-draft-a-backlog.md)** — use AI to draft a backlog from a feature brief, then grade and fix it against INVEST. *(~1h)*

## Before you start

- You've read all three lectures.
- You loaded this week's seed (`board_items`, `jira_issues`) from the [week README](../README.md) — `SELECT COUNT(*) FROM board_items;` returns **16**, `SELECT COUNT(*) FROM jira_issues;` returns **14**.
- You have a free GitHub account and `gh` installed and authenticated (`gh auth login`) for Exercise 1.
- You have access to a Jira Cloud site (a free trial/sandbox is enough — see [`resources.md`](../resources.md)) for Exercise 2's live-JQL portion, or you can work entirely against the seed if you'd rather skip account setup this week.
- You have access to an AI assistant for Exercise 3.

## Suggested workflow

- Exercises 1 and 2 have a **live-tool** half (do it in the real product) and a **SQL** half (reproduce it against the seed). Do both — the point is building the translation reflex between "I did this by clicking" and "I did this with a query," because in a real job you'll need both.
- Keep every query and script you write — Exercise 1's export script and Exercise 2's JQL/SQL pairs both get reused directly in this week's challenges and mini-project.
- For Exercise 3, resist the urge to accept the AI's first draft. The exercise isn't complete until you've applied INVEST and rewritten at least one story.
