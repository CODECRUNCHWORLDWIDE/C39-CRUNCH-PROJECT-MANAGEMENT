# Lecture 1 — GitHub Projects, In Depth

> **Duration:** ~2 hours. **Outcome:** You can configure a GitHub Projects (v2) board's views, custom fields, and iterations from scratch, wire built-in automations so it maintains itself, and export the whole board into a SQL table you can query — all without a single spreadsheet.

Marcus inherited Atlas's GitHub Projects board six weeks ago from whoever set it up in twenty rushed minutes before a demo. It has one view (a table, unsorted), one custom field (a `Priority` select field three people forgot exists), and zero automations — every status change is a human dragging a card. That's not a tool failure; it's a **configuration** failure, and it's the single most common state a real GitHub Projects board is in. This lecture fixes that, field by field.

## 1. What a GitHub Project actually is

A **GitHub Project** (the current version is "Projects v2" — if you see the term "Projects (beta)" in older docs, it's the same thing, now GA) is **not** a repository feature — it's an org- or user-level object that *references* issues and pull requests from one or more repos. This matters:

- One project can span **multiple repositories**. Atlas's board could track issues from `northlight/atlas`, `northlight/atlas-mobile`, and `northlight/atlas-infra` in one place.
- An issue can belong to **multiple projects** at once (e.g., a team board and a company-wide roadmap board).
- The project itself stores **project-only data** — the custom field values (status, priority, size, iteration) — that don't exist on the issue itself. This is why the same issue can show a different "Status" on two different boards: status lives on the *project item*, not the issue.

A **project item** is the row you see on the board. It wraps either an **Issue**, a **Pull Request**, or a **Draft issue** (a project-only placeholder with no linked issue yet — useful for capturing an idea before it's ready to be a real GitHub issue). `board_items.item_type` in this week's seed captures exactly that distinction; row 16, "Explore an AI backlog-grooming assistant," is a draft issue with no `issue_number` because nobody has promoted it to a real issue yet.

## 2. Custom fields — the vocabulary of your board

Out of the box, a new project has exactly one field: **Status**, a single-select with `Todo` / `In Progress` / `Done`. Everything else — the fields that make a board actually answer questions — you add yourself. GitHub Projects supports six field types:

| Field type | Example | Notes |
|---|---|---|
| Single select | `Priority`: P0/P1/P2/P3 | The workhorse type; drives grouping and colored labels on cards |
| Number | `Size` (if you use points instead of T-shirt sizes) | Sorts and sums numerically |
| Text | A free-text note field | Rarely a good idea — unstructured data doesn't filter or group |
| Date | `Target date` | Combine with a saved view filtered to "overdue" |
| Iteration | `Sprint` / `Iteration` | GitHub-native sprint concept — a repeating date range with a name |
| Single select (labels-backed) | Mirrors repo labels | Auto-populates from the issue's GitHub labels |

Atlas's board (this week's seed) uses four: **Status** (Todo/In Progress/In Review/Blocked/Done — the built-in field extended with two extra options), **Priority** (P0–P3, a single select Marcus adds this week), **Size** (XS/S/M/L/XL, a single select — some teams use a Number field with story points instead; T-shirt sizes are common when a team explicitly *doesn't* want to argue about whether something is a 3 or a 5), and **Iteration** (Sprint 7, Sprint 8 — GitHub's native Iteration field, which auto-advances the "current iteration" as dates roll forward).

To add one via the UI: open the project → the `+` at the right edge of the field header row → **New field** → pick the type → name it → for single-select, add your options with colors. Via the GraphQL API (what you'll script in Exercise 1), it's a `createProjectV2Field` mutation — more on that below.

**A field-design rule worth internalizing:** every field should answer a question someone actually asks. `Priority` answers "what do we do first if we can't do everything." `Size` answers "how much of this sprint does this eat." A field nobody filters or groups by is decoration, not data — and decoration fields are exactly what rot into "P2 on everything" the way Atlas's old `Priority` field did.

## 3. Views — the same items, sliced differently

A **view** is a saved combination of layout + filter + sort + grouping + visible fields. A project can have many views; switching views doesn't change the underlying data, only how you're looking at it. Three layouts matter for this week:

- **Table** — a spreadsheet-like grid, one row per item, columns = fields. Best for bulk-editing (select multiple rows, set a field on all of them at once) and for the view you'd export from.
- **Board** — the classic kanban columns, grouped by a single-select field (almost always `Status`). Best for a stand-up: "what's In Progress right now."
- **Roadmap** — a timeline, driven by Date or Iteration fields. Best for the kind of "when will this ship" conversation Priya asks Marcus in steering meetings.

For Atlas, Marcus keeps three saved views: **"Sprint board"** (Board layout, grouped by Status, filtered to `iteration: @current`), **"Everything"** (Table layout, no filter — the export source), and **"Blocked & P0"** (Table layout, filtered to `status: Blocked OR priority: P0`, sorted by `updated_at` ascending — the oldest blockers surface first, which is exactly the query you'd write in SQL as `ORDER BY updated_at ASC`).

Filters use GitHub's project filter syntax, which will look familiar if you've used GitHub's issue search:

```
status:"In Progress" priority:P0,P1
is:issue -label:tooling
```

That reads: status is In Progress, priority is P0 *or* P1, item is an issue (not a PR), and it does **not** have the `tooling` label. Multiple values on one field are OR'd (`P0,P1`); different fields are AND'd; `-` negates.

## 4. Iterations — GitHub's native sprint field

The **Iteration** field type is purpose-built for sprints: you configure a start date and a duration (e.g., every 2 weeks), and GitHub generates a rolling series of named iterations (`Sprint 7`, `Sprint 8`, …) automatically — no manual date bookkeeping every two weeks. Two things make it more useful than a plain single-select doing the same job:

1. **`@current`** — every saved view and every filter can reference the iteration field's current value without hardcoding a name, so "Sprint board" never needs editing when Sprint 9 starts.
2. **Duration-aware grouping** — the Roadmap view can render each iteration as a labeled band, so a sprint boundary is visually obvious, not just a filter you have to remember to apply.

Set this up once (**Settings → Fields → Iteration → configure start date + duration**) and every future sprint inherits it. This is the single highest-leverage five minutes Marcus never spent on the old board — he was hand-editing a `Sprint` text field every two weeks, which is exactly the kind of manual, error-prone busywork automation exists to kill.

## 5. Built-in automations (workflows)

GitHub Projects ships several **workflows** you turn on per-project under **Settings → Workflows** — no scripting required for the common cases:

| Workflow | Trigger | Effect |
|---|---|---|
| **Item added to project** | An issue/PR is added (manually or via auto-add) | Sets Status to a field value you choose (usually `Todo`) |
| **Item reopened** | A closed issue/PR reopens | Sets Status (usually back to `Todo` or `In Progress`) |
| **Item closed** | An issue/PR closes | Sets Status to `Done` |
| **Pull request merged** | A linked PR merges | Sets Status to `Done` |
| **Auto-add to project** | An issue/PR is created (or a label is applied) matching a filter | Adds it to the project automatically |
| **Auto-archive items** | An item has been in a closed/Done state for N days | Archives it off the active board (keeps history, declutters the view) |

Atlas turns on three this week (this is the substance of board_items rows 8, 11, and 13 — all `tooling,automation`–labeled items Marcus closes on Monday): **auto-add** any issue opened in `northlight/atlas` with the label `atlas-team` (so nobody has to remember to drag new issues onto the board), **item closed → Status: Done** (so closing an issue on GitHub and marking it Done on the board are the same action, not two), and **auto-archive** anything Done for more than 14 days (so the "Everything" table view doesn't accumulate nine months of closed noise).

For anything a built-in workflow can't express — "when Priority is set to P0, also set Status to In Progress and ping the on-call channel" — you reach for the **GraphQL API** and a small script (or GitHub Actions using `actions/github-script`), which is exactly what Exercise 1's automation task does.

## 6. Wiring a repo's issues into a project (the GraphQL you actually need)

The UI is fine for one-off changes; scripting is how you do anything repeatable — including this week's export. GitHub Projects v2 is **GraphQL-only** for programmatic access (the older REST Projects API is deprecated for v2 boards). Three calls matter:

**Find the project's node ID** (you need this for every later call):

```graphql
query {
  organization(login: "northlight") {
    projectV2(number: 4) {
      id
      title
    }
  }
}
```

**Add an issue to the project:**

```graphql
mutation {
  addProjectV2ItemById(input: {
    projectId: "PVT_kwDOexampleprojectid"
    contentId: "I_kwDOexampleissueid"
  }) {
    item { id }
  }
}
```

**Set a custom field value on an item** (single-select fields take the *option ID*, not the label text — fetch the field's options first with a query on `field { ... on ProjectV2SingleSelectField { options { id name } } }`):

```graphql
mutation {
  updateProjectV2ItemFieldValue(input: {
    projectId: "PVT_kwDOexampleprojectid"
    itemId: "PVTI_exampleitemid"
    fieldId: "PVTSSF_examplefieldid"
    value: { singleSelectOptionId: "f75ad846" }
  }) {
    projectV2Item { id }
  }
}
```

You rarely hand-write raw GraphQL like this day to day — the `gh project` CLI subcommands (`gh project item-add`, `gh project field-list`, `gh project item-edit`) wrap the common cases. But knowing the underlying calls matters the moment you need something the CLI doesn't cover, like bulk-editing 40 items from a script, which is exactly Exercise 1.

## 7. Exporting the board into SQL — why, and how

Here is where this week's data-tooling rule earns its keep. GitHub's own UI can export a view to CSV — but a CSV you glance at once and discard answers today's question and no others. What Atlas actually needs is *this sprint's velocity compared to last sprint's*, *how long P0 items sit before someone picks them up*, *which engineer has the most Blocked items right now* — all questions that require the data to persist and be queryable, which is exactly `board_items` in this week's README.

The real pipeline, which Exercise 1 has you run:

```bash
# 1. Pull every item on the project as JSON via gh's GraphQL wrapper
gh api graphql -f query='
  query($org: String!, $number: Int!) {
    organization(login: $org) {
      projectV2(number: $number) {
        items(first: 100) {
          nodes {
            content { ... on Issue { number title } ... on PullRequest { number title } }
            fieldValues(first: 20) {
              nodes {
                ... on ProjectV2ItemFieldSingleSelectValue { field { ... on ProjectV2FieldCommon { name } } name }
                ... on ProjectV2ItemFieldTextValue { field { ... on ProjectV2FieldCommon { name } } text }
                ... on ProjectV2ItemFieldDateValue { field { ... on ProjectV2FieldCommon { name } } date }
              }
            }
          }
        }
      }
    }
  }' -f org=northlight -F number=4 > board_export.json
```

```python
# 2. Flatten the nested JSON into rows and load with pandas + SQLAlchemy
import json
import pandas as pd
from sqlalchemy import create_engine

with open("board_export.json") as f:
    raw = json.load(f)

items = raw["data"]["organization"]["projectV2"]["items"]["nodes"]

rows = []
for item in items:
    row = {"issue_number": (item.get("content") or {}).get("number"),
           "title": (item.get("content") or {}).get("title")}
    for fv in item["fieldValues"]["nodes"]:
        name = fv.get("field", {}).get("name")
        if not name:
            continue
        row[name] = fv.get("name") or fv.get("text") or fv.get("date")
    rows.append(row)

df = pd.DataFrame(rows)
engine = create_engine("postgresql://localhost/atlas_pm")
df.to_sql("board_items_raw", engine, if_exists="replace", index=False)
```

```sql
-- 3. Now it's queryable like anything else this course has taught
SELECT status, COUNT(*) AS item_count
FROM board_items
GROUP BY status
ORDER BY item_count DESC;
```

Run that last query against this week's seed and you'll see 5 statuses, `Done` the largest group at 8 — the shape of a healthy sprint that's roughly half-closed on day one of Sprint 8 (those 8 are carried-over Sprint 7 closes, not Sprint 8 work — a distinction `GROUP BY status, iteration` makes precise, and precisely the kind of question a CSV you eyeballed once would never let you re-ask next week).

## 8. Check yourself

- What's the difference between a project *item* and the issue or PR it wraps?
- Name the six GitHub Projects custom field types. Which one is Atlas's `Priority` field?
- Why does the Iteration field's `@current` value matter more than it looks like it should?
- Which built-in workflow would you turn on so that merging a PR automatically marks its item Done?
- Why is the GraphQL API required (not the REST API) for scripting against a Projects v2 board?
- Run `SELECT status, COUNT(*) FROM board_items GROUP BY status;` against this week's seed before moving on — can you explain each number from the raw rows?

## Further reading

- **GitHub Docs — "About Projects":** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects>
- **GitHub Docs — "Automating Projects":** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-built-in-automations>
- **GitHub Docs — "Using the API to manage Projects" (GraphQL):** <https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-api-to-manage-projects>
- **`gh project` CLI reference:** <https://cli.github.com/manual/gh_project>
