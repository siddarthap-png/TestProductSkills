---
name: prd-stitch-ux
description: >-
  Drives UX generation in Google Stitch from a Product Requirements Document.
  Asks for a PRD link if missing, ingests the PRD, derives screens and flows,
  then runs the Stitch MCP workflow (project, design system, screens). Use when
  the user mentions PRD, product requirements, spec-driven UX, generate UI from
  requirements, Stitch from PRD, or wants designs aligned to a requirements doc.
---

# PRD → Google Stitch UX

## Purpose

Treat the **PRD as source of truth** for **what** to build; use **Stitch** for **visual UX**. This skill applies whenever UX work should be **grounded in written requirements**, not only in ad-hoc prompts.

## When invoked

1. **PRD link is mandatory for a full run.** If the user has not provided one, **ask immediately**:
   - *“Please share the **PRD link** (Google Doc, Notion, Confluence, GitHub, PDF URL, or paste the doc). I’ll use it as context for Stitch.”*
2. If the user only pastes PRD **text** (no URL), treat that as the PRD body and note `PRD source: pasted` in the session summary.
3. If the user refuses a PRD but wants Stitch anyway, fall back to a normal Stitch prompt **without** claiming PRD traceability.

## Workflow (ordered)

### 1. Ingest PRD

- **Fetch** the link when possible (`web_fetch`, repo file read, or export). If access fails, ask the user to **paste** the relevant sections or export a file.
- **Extract** (bullet notes the agent keeps for prompts):
  - **Product / feature name** and **goal**
  - **Primary users** and **jobs-to-be-done**
  - **User flows** (step lists) and **explicit screen / page list**
  - **Must-have UI elements**, validations, empty/error states called out in the PRD
  - **Out of scope** and **dependencies** (e.g. auth, payments) — avoid designing wrong surfaces
- **Map** each PRD screen/flow step to **one or more Stitch generations** (start with **highest-priority** flow).

### 2. Align with design process skills

- If the product is **redBus / redRail** (or user points to it), read and follow **[redbus-design-language](../redbus-design-language/SKILL.md)** and pin **[STITCH.md](../redbus-design-language/STITCH.md)** content into the Stitch **design system** `designMd` (or equivalent) so tokens and mWeb defaults match.
- Otherwise: build a short **designMd** from the PRD (tone, density, accessibility notes) plus any **brand tokens** the PRD supplies.

### 3. Stitch MCP (check schemas first)

Before any `call_mcp_tool` to **user-stitch**, read the tool descriptor JSON under the workspace MCP folder (e.g. `mcps/user-stitch/tools/<tool>.json`) so arguments match the current API.

**Typical sequence:**

1. **`create_project`** — `title` from PRD feature name (e.g. `PRD: {feature} — UX`).
2. **`create_design_system`** — `projectId` (numeric id from `projects/{id}`), `designSystem.displayName`, `designSystem.theme` with required fields per schema; put a **concise PRD summary + screen list + design rules** in **`designMd`** (truncate if needed; prioritize flows and constraints).
3. **`update_design_system`** — same `name` (e.g. `assets/{assetId}`) and `projectId` as returned; same theme payload if the tool instructions require it to apply in UI.
4. **`generate_screen_from_text`** — **once per screen** (or batched per user preference): `projectId`, `deviceType` **`MOBILE`** + prompt text **“mobile web (mWeb)”** unless the PRD explicitly requires desktop/native; `prompt` must cite **PRD-derived** layout, copy hints, and acceptance-relevant states. **Do not retry** the same generation on failure (Stitch may still complete); optionally `get_screen` later per schema.

Use **`modelId`** per schema if you need a specific model. Allow **several minutes** per `generate_screen_from_text` call.

### 4. Deliverables back to the user

- **PRD link** (or “pasted”) and **feature title**
- **Stitch project id** and **project title**
- **List of screens generated** mapped to **PRD sections** (traceability)
- **Gaps**: PRD items that could not be generated (missing access, ambiguous spec)
- **Next steps**: e.g. add screenshots to Stitch, second pass for edge cases

## Quality checks

- [ ] PRD link or pasted content recorded before claiming PRD-grounded output.
- [ ] Generated prompts **quote or paraphrase** PRD requirements, not generic marketing UI.
- [ ] Out-of-scope items from the PRD are **not** designed as full flows unless asked.
- [ ] Stitch tool schemas **read** before MCP calls; no invented parameter names.

## Related skills

- **redBus visual language:** [redbus-design-language](../redbus-design-language/SKILL.md)
- **Stitch tool details and PRD → designMd template:** [reference.md](reference.md)
