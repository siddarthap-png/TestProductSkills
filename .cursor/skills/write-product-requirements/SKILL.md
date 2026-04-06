---
name: write-product-requirements
description: >-
  Authors a Product Requirements Document (PRD) after clarifying why the document
  exists. Prompts for **purpose** and **audience** first; drafts a structured PRD
  (problem, goals, scope, users, requirements, metrics, risks), then delivers the
  **final artifact as a Microsoft Word file (.docx)**—not Markdown alone. Use when
  the user asks for a PRD, product requirements doc, feature spec, requirements
  document, Word PRD, or .docx requirements before design or build.
---

# Write Product Requirements (PRD)

## Purpose of this skill

Guide the agent to produce a **clear, reviewable PRD** without skipping **why the document exists**. The PRD is written **after** purpose is understood—not before.

**Deliverable rule:** The user-facing output is a **Microsoft Word document (`.docx`)**. The agent may draft in Markdown **only as an intermediate step**; it must **finish by producing a `.docx`** in the user’s project (or explicit path), unless the user **explicitly** asked for Markdown-only.

## When invoked

Apply this skill when the user wants a **Product Requirements Document**, **feature spec**, or similar, **unless** they already pasted a full template and only want formatting edits.

## Workflow (strict order)

### 1. Purpose gate — do this first

**Do not** write the full PRD until the **purpose of the document** is clear.

If the user has **not** already stated all of the following, ask in **one** concise message (adapt wording; keep the intent):

1. **Purpose** — Why is this PRD being written? (e.g. align engineering on v1, stakeholder sign-off, input for design or Stitch, roadmap prioritization, vendor scope.)
2. **Primary audience** — Who must read and act on it? (e.g. engineering, design, leadership, partners.)
3. **Desired outcome** — What should readers **decide or do** after reading? (e.g. approve scope, estimate effort, build a defined backlog.)

**Optional** follow-ups only if still ambiguous: product/feature name, deadline or release target, hard constraints (platform, compliance), explicit **out of scope** hints.

If the user says **“use sensible defaults”** or **“proceed”** after a partial answer, capture stated assumptions in an **Assumptions** subsection of the PRD and continue.

### 2. Draft the PRD

Once purpose (and enough context) is clear:

1. Use the **[PRD template](#prd-template-markdown)** below.
2. Tie the **Executive summary** and **Document purpose** sections directly to what the user said in step 1.
3. Prefer **specific, testable** requirements; use **`TBD`** with a short note where information is missing—do not invent facts.
4. Keep **non-goals** explicit to prevent scope creep.
5. Write the draft to **`PRD-<short-product-or-feature-name>.md`** in the project (or path the user gave), then **convert to Word** per **[Microsoft Word (.docx) deliverable](#microsoft-word-docx-deliverable)**.

### 2b. Microsoft Word (.docx) deliverable

The **canonical PRD file** is **`PRD-<short-product-or-feature-name>.docx`** (same base name as the `.md` draft unless the user specified otherwise).

**Conversion order (try in sequence):**

1. **Pandoc** (strong fidelity for headings, lists, tables from Markdown):  
   `pandoc "PRD-<name>.md" -o "PRD-<name>.docx"`  
   If `pandoc` is missing, suggest install: [Pandoc install](https://pandoc.org/installing.html) (Windows: `winget install --id JohnMacFarlane.Pandoc` or MSI).

2. **Python helper (bundled with this skill)** — If `pandoc` is not installed or fails:  
   `python scripts/md_to_docx.py "PRD-<name>.md" "PRD-<name>.docx"`  
   Run with `scripts/` resolved to **`write-product-requirements/scripts/md_to_docx.py`** (copy this skill folder into the project’s `.cursor/skills/` as usual). One-time dependency: `pip install python-docx`. Supports headings, tables, bullets, `**bold**`, `*italic*`.

3. **Microsoft Word (GUI)** — If Word is installed: **File → Open** the `.md` file, then **File → Save As** → **Word Document (`*.docx`)**. (Minor table tweaks may be needed.)

4. **Copy-paste fallback:** Paste the rendered Markdown body into a **new blank Word document**, apply **Heading 1 / Heading 2**, recreate **tables** with **Insert → Table**, then **Save As `.docx`**.

After a successful `.docx` is created, **tell the user the full path** to the Word file. Optionally keep the `.md` alongside for version control; if the user wants **only** `.docx`, do not delete the `.md` without their say-so (see team git rules elsewhere).

**Agents:** Prefer **pandoc** when available; otherwise run **`md_to_docx.py`** after `pip install python-docx`; verify the output file exists after conversion.

### 3. Handoff

- If the user will use **Google Stitch** or PRD-driven UX next, mention they can use **[prd-stitch-ux](../prd-stitch-ux/SKILL.md)** with this PRD as input.

## PRD template (Markdown)

Use these headings unless the user asks for a different structure (e.g. company-mandated PRD format). Omit sections that do not apply; never omit **Document purpose** or **Problem / opportunity**.

```markdown
# <Product or feature name> — Product Requirements

| Field | Value |
|-------|--------|
| **Version** | e.g. 0.1 DRAFT |
| **Last updated** | YYYY-MM-DD |
| **Owner** | TBD |
| **Status** | Draft / In review / Approved |

## Document purpose

- **Why this PRD exists:** <1–3 sentences from purpose gate>
- **Primary audience:** <who>
- **Success for this document:** <what readers decide or do>

## Executive summary

<3–6 sentences: problem, proposed direction, why now.>

## Problem / opportunity

- **Current state:** <pain, gap, or market opportunity>
- **Evidence or signals:** <data, research, requests—TBD if unknown>

## Goals

| Goal | Measurable indicator (if known) |
|------|----------------------------------|
| … | … |

## Non-goals (out of scope)

- …

## Users and use cases

- **Primary users:** …
- **Key scenarios:** numbered list of jobs-to-be-done or flows at a high level

## Functional requirements

| ID | Requirement | Priority (Must / Should / Could) | Notes |
|----|-------------|-----------------------------------|-------|
| FR-01 | … | Must | … |

## Non-functional requirements

| ID | Category (e.g. performance, security, accessibility) | Requirement | Notes |
|----|------------------------------------------------------|-------------|-------|
| NFR-01 | … | … | … |

## User experience / design notes (optional)

- Key surfaces, flows, or principles—**no pixel-perfect spec** unless user supplied it.

## Dependencies and assumptions

- **Dependencies:** other teams, systems, vendors
- **Assumptions:** include “sensible default” assumptions if user asked to proceed with incomplete info

## Risks and open questions

| Risk or question | Impact | Owner / next step |
|------------------|--------|-------------------|
| … | … | … |

## Milestones or phases (optional)

- Phase 1: …
- Phase 2: …

## Appendix: glossary (optional)

- **Term:** definition
```

## Quality checks

- [ ] Purpose gate satisfied: document purpose and audience reflect the user’s answers.
- [ ] Goals and non-goals are both present where applicable.
- [ ] Requirements are **numbered or ID’d** and **prioritized** where possible.
- [ ] Unknowns are **`TBD`** or in **Open questions**, not silently guessed as facts.
- [ ] **A `.docx` exists** at the agreed path (or default `PRD-<name>.docx`), produced via pandoc, **`md_to_docx.py`**, Word Save As, or paste fallback—not **only** a `.md` file unless the user opted out of Word.

## Related skills

- **PRD → Stitch UX:** [prd-stitch-ux](../prd-stitch-ux/SKILL.md)
