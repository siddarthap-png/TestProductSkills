---
name: jira-sheet-sync
description: >-
  Syncs JIRA.xlsx (Sheet1): creates only Jira Story issues via MCP (user-jira /
  jira_create_issue), maps Acceptance criteria to mandatory REDRAIL field when configured,
  writes updates.json, applies Status Done and JIRA ID. Use for CursorProject/JIRA,
  JIRA sheet sync, or create-from-sheet.
---

# Jira sync from `JIRA.xlsx` (MCP-only create, **Story only**)

## Scope

- **Project root**: `C:/Users/siddartha.p/Desktop/CursorProject/JIRA` (this repo).
- **Workbook**: **`JIRA.xlsx`**, sheet **`Sheet1`** (optional `JIRA_XLSX_PATH` / `JIRA_SHEET_NAME` in `.env`).
- **GitHub (published skill)**: [siddarthap-png/TestProductSkills](https://github.com/siddarthap-png/TestProductSkills) — `.cursor/skills/jira-sheet-sync/SKILL.md` on `main`.
- **Issue creation**: **Only via the Jira MCP** (`user-jira`, tool **`jira_create_issue`**). Do **not** create issues with Node `fetch` / REST from this repo.

## Mandatory: every new issue is a **Story**

| Rule | Detail |
|------|--------|
| **`issue_type`** | Export **always** sets **`mcp.arguments.issue_type`** to **`Story`**. Do not create **Improvement**, **Task**, or **Bug** from this workflow unless the user explicitly changes the script. |
| **MCP** | Call **`jira_create_issue`** with **`issue_type`: `"Story"`** exactly as in **`mcp.arguments`**. |
| **Excel `Type` column** | Optional for humans (e.g. label **Story**). It is **not** used to switch issue type — sync policy is **Story only**. |

## **`Acceptance criteria` column (Excel) → Jira mandatory field**

`JIRA.xlsx` **row 1** must include a column for acceptance text. **Canonical header:** **`Acceptance criteria`** (case-insensitive). The same cell value is used in two places:

1. **Jira description** — appended as a `### Acceptance criteria` Markdown section (for readers).
2. **`additional_fields`** — copied into the REDRAIL **Acceptance Criteria** custom field id from **`.env`** (`JIRA_ACCEPTANCE_CRITERIA_FIELD_ID`). This is what Jira enforces for **Story** create; description alone is not enough.

| Requirement | Detail |
|-------------|--------|
| **`.env`** | Set **`JIRA_ACCEPTANCE_CRITERIA_FIELD_ID`** to the Jira field id (e.g. **`customfield_10046`**). Find it: **Project settings → Fields → Acceptance Criteria → …** or ask a Jira admin. |
| **Excel** | **`Acceptance criteria`** column must be **non-empty** for each **Status = New** row you expect to create. |
| **Aliases** | **`AC`**, **`UAC`**, **`User acceptance criteria`**, **`Acceptance`**, **`Acceptance criteria (redrail)`**, and the misnamed header **`JIRA_ACCEPTANCE_CRITERIA_FIELD_ID`** (column still holds **AC text**; the Jira custom field id belongs only in **`.env`**). See **`acceptanceCriteriaHeaders`** in export JSON. Prefer renaming the Excel column to **`Acceptance criteria`** to avoid confusion. |
| **Export** | Puts acceptance text into **`additional_fields`** under that custom field id as **Atlassian Document (ADF)** JSON (Jira Cloud requires ADF for this field), **merged** with **Priority** in one JSON object. |
| **`acceptanceColumnDetected`** | Export includes **`true`**/`false`**. If **`false`**, fix row-1 headers (see **`sheetWarning`**). |
| **`configWarning`** | If **`JIRA_ACCEPTANCE_CRITERIA_FIELD_ID`** is missing, export includes **`configWarning`**; Story create will fail until set. |

**Agent behavior:** Pass **`additional_fields`** exactly as exported (stringified JSON). If MCP returns “Acceptance Criteria is mandatory”, verify **`.env`**, **`acceptanceColumnDetected`**, and non-empty **Acceptance criteria** cells (not merged across rows, not hidden-only).

## Business rules (must follow)

1. Act only on rows where **Status** equals **`New`** (case-insensitive after trim).
2. **Create** one **new** Jira **Story** per qualifying row using **MCP** (skip if **JIRA ID** already holds a real key like `REDRAIL-12345`).
3. **After successful MCP create**: set **Status** to **`Done`**, **JIRA ID** to the **returned issue key**.
4. **If create fails** or **Summary** is empty: set **JIRA ID** to **`NA`** only; do **not** set Status to Done unless a ticket was created.

## Column names (case-insensitive)

Headers are matched after **trim + lower-case + normalize spaces**. The export script maps columns into **`mcp.arguments`**. Re-run **`npm run create-from-sheet`** after sheet changes.

### Canonical layout (current `JIRA.xlsx` row 1)

| Excel header | Role |
|--------------|------|
| **`Project`** | **`project_key`**. Aliases: **`JIRA LOB`**, **`LOB`**. |
| **`Summary / title`** | Jira **summary**. Aliases: **`Summary`**, **`Title`**, **`Name`**; else **`Description`** (truncated). |
| **`Description`** | Markdown **description** (with acceptance + links sections). |
| **`Acceptance criteria`** | **Required** for REDRAIL Story: drives **`### Acceptance criteria`** in description **and** the mandatory custom field in **`additional_fields`** (needs **`JIRA_ACCEPTANCE_CRITERIA_FIELD_ID`**). Aliases: see **REDRAIL** section above. |
| **`Links`** | Appended under **Links** in **description**. Legacy attachment columns still work. |
| **`Priroity`** / **`Priority`** | **`additional_fields.priority`**. |
| **`Type`** | Informational only; **does not change issue type** (always **Story**). |
| **`Status`** | **`New`** to queue. |
| **`JIRA ID`** | Output. Aliases: **`Jira Key`**, **`Issue Key`**, **`Key`**. |

## MCP: create issue

- **Server**: `user-jira`
- **Tool**: `jira_create_issue`
- **Arguments**: Use **`pending[].mcp.arguments`** verbatim — includes **`issue_type": "Story"`**, **`additional_fields`** (priority + acceptance custom field when configured).

Example shape:

```json
{
  "project_key": "REDRAIL",
  "summary": "<from sheet>",
  "issue_type": "Story",
  "components": "Android",
  "description": "<Markdown>",
  "additional_fields": "{\"priority\":{\"name\":\"P1\"},\"customfield_XXXXX\":\"<Acceptance criteria text>\"}"
}
```

If MCP returns **IP allowlist** errors, use an allowed network (e.g. VPN).

## Execution order (agent must do all steps)

1. **Export** — `npm run create-from-sheet` (check **`configWarning`**, **`sheetWarning`**, **`acceptanceColumnDetected`**).
2. **MCP** — For each **`pending[].mcp`**, call **`jira_create_issue`** with exported **`arguments`** (Story + **`additional_fields`**).
3. **`updates.json`** — `{ "updates": [ { "row": N, "jiraKey": "REDRAIL-…" } ] }`.
4. **Apply** — Close Excel → `node create-jira-from-sheet.js --apply updates.json`.

## Workflow in Cursor (summary)

| Step | Action |
|------|--------|
| 1 | `npm run create-from-sheet` → **`issue_type` is always Story** |
| 2 | MCP **`jira_create_issue`** → collect **`issue.key`** |
| 3 | **`updates.json`** |
| 4 | **`--apply`** |

## Jira defaults (this org)

- **Issue type**: **Story only** (export).
- **Project**: **`Project`** column or **`JIRA_PROJECT_KEY`** in `.env`.
- **Acceptance Criteria field**: **`JIRA_ACCEPTANCE_CRITERIA_FIELD_ID`** in `.env`.
- **Component**: **Android**.

## Do not

- Create issues with **Node/fetch/REST** in this workflow unless the user explicitly overrides.
- Change **`issue_type`** away from **Story** when following this skill unless the user explicitly asks for a different policy.
- Mark **Done** or write a real key when creation did not succeed.
