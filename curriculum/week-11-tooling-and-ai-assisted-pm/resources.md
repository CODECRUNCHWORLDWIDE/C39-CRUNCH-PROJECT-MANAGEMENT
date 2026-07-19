# Week 11 — Resources

Free, public, no signup unless noted. Read the "required" set; treat the rest as reference you dip into when a specific question comes up.

## Install / sign up first

- **GitHub account** (free) — <https://github.com/join> — you likely already have one from earlier weeks.
- **`gh`, the GitHub CLI** — <https://cli.github.com/> — macOS: `brew install gh`. Linux: see the site's per-distro instructions. Windows: `winget install --id GitHub.cli`. After install, run `gh auth login` once.
- **Jira Cloud free/trial site** (optional, for live JQL practice) — <https://www.atlassian.com/software/jira/free> — free tier supports up to 10 users, plenty for this week. You can complete every exercise against this week's seed data alone if you'd rather skip account setup.
- **PostgreSQL 16+** or **SQLite 3.35+** — already installed from earlier weeks (see Week 8's `resources.md` if you need it fresh).
- **Python 3.10+ with pandas** — `pip install pandas sqlalchemy requests`.
- **An AI assistant** — any capable general-purpose one (Claude, ChatGPT, etc.) works for Lecture 3 and its exercises/challenges.

## Required reading (this week's core)

- **GitHub Docs — "About Projects":** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects>
  *Why: the canonical explanation of items, fields, and views — read before Lecture 1's Section 2–3.*
- **GitHub Docs — "Automating your project" (built-in workflows):** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-built-in-automations>
  *Why: the exact reference for every automation Exercise 1 has you configure.*
- **Atlassian — "Advanced search reference — JQL fields":** <https://support.atlassian.com/jira-software-cloud/docs/advanced-search-reference-jql-fields/>
  *Why: the field-by-field JQL reference you'll want open during Exercise 2.*
- **Atlassian — "Configure workflows":** <https://support.atlassian.com/jira-cloud-administration/docs/configure-workflows/>
  *Why: how Jira's statuses-and-transitions model actually gets built, referenced in Lecture 2 Section 3 and Challenge 1.*
- **Anthropic — "Reducing hallucinations":** <https://docs.claude.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations>
  *Why: the official grounding guidance Lecture 3's three-stage pattern is built on.*

## Reference (keep in tabs)

- **GitHub — "Using the API to manage Projects" (GraphQL):** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-api-to-manage-projects>
  *Why: every mutation/query used in Lecture 1 Section 6–7, spelled out in full.*
- **`gh project` CLI reference:** <https://cli.github.com/manual/gh_project>
  *Why: the command-line shortcuts that wrap the common GraphQL calls — use these before hand-writing GraphQL when the CLI already covers your case.*
- **GitHub GraphQL API Explorer:** <https://docs.github.com/en/graphql/overview/explorer>
  *Why: an interactive sandbox to test a GraphQL query against your real account before wiring it into a script.*
- **Atlassian — Jira Cloud REST API, `/search`:** <https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-search/>
  *Why: the endpoint Lecture 2 Section 6's export script calls; check the exact response shape here.*
- **Atlassian — "Scrum vs. Kanban":** <https://www.atlassian.com/agile/kanban/kanban-vs-scrum>
  *Why: a clean refresher that pairs with Week 2 of this course.*
- **Anthropic — "Long context prompting tips":** <https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/long-context-tips>
  *Why: technique for feeding a large, structured JSON block (Stage 2) into a prompt without the model losing track of it.*

## Practice beyond the seed data

- **GitHub Skills — "Manage projects with GitHub Projects":** <https://skills.github.com/> (search for the Projects course)
  *Why: a free, hands-on GitHub-hosted course that walks a real repo through project setup, step by step.*
- **Atlassian University — free Jira fundamentals courses:** <https://university.atlassian.com/student/collection/641968-jira-cloud>
  *Why: short, free, official video lessons if you want a second pass on Jira's UI beyond this week's lectures.*

## Concept map (Jira ↔ GitHub Projects, at a glance)

| Jira | GitHub Projects | Notes |
|------|------------------|-------|
| Project | Repository (loosely) | A GitHub Project itself can span multiple repos — not a 1:1 match |
| Issue | Issue | Direct match |
| Epic | Sub-issues (parent issue) | Structurally similar, repo-level not project-level; see Challenge 1 |
| Board (Scrum) | View (Board layout) + Iteration field | Sprints are native to Scrum boards; Iteration field replicates the concept |
| Board (Kanban) | View (Board layout), no Iteration | Continuous flow, no sprint ceremony either side |
| Workflow (statuses + transitions) | Status single-select | GitHub has no native transition enforcement — see Lecture 2, Section 3 |
| Sprint | Iteration field value | Both are dated, named, repeating |
| Story points (custom field) | Size / Number field | No forced mapping — Challenge 1 makes you choose and justify one |
| Rank (manual drag order) | No equivalent | Not derived from any exported field; see Exercise 2, Part C |
| JQL | Project filter syntax | Same job (a `WHERE` clause), different syntax |
| Saved filter | Saved view | Both back a board/view with a reusable query |

## Glossary

| Term | Definition |
|------|------------|
| **Project item** | A row on a GitHub Projects board — wraps an Issue, Pull Request, or Draft issue. |
| **Custom field** | A project-level data column (single-select, number, date, iteration, text) not stored on the issue itself. |
| **Iteration field** | GitHub's native sprint-like field type; auto-generates a rolling, named, dated series. |
| **Workflow (GitHub)** | A built-in automation rule (e.g., "item closed → Status: Done") configured per project. |
| **Workflow (Jira)** | The full statuses-and-transitions graph governing what status changes an issue can legally make. |
| **JQL** | Jira Query Language — Jira's structured search syntax, the JQL analog of SQL's `WHERE`. |
| **Custom field ID (Jira)** | An instance-specific numeric field name (`customfield_10016`) the REST API uses for non-standard fields like story points. |
| **Grounded prompt** | A prompt whose every stated fact traces to data retrieved (Stage 1) and serialized (Stage 2) before the model ever sees it. |
| **Fabrication / hallucination** | A model-generated claim (a number, date, or fact) with no basis in the provided data. |
| **INVEST** | Week 3's backlog-story quality bar: Independent, Negotiable, Valuable, Estimable, Small, Testable. |

---

*Broken link? Open an issue or PR.*
