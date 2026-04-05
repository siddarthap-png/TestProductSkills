# PRD → Stitch — reference

## PRD extraction checklist (for prompts)

Use this to turn a PRD into Stitch-ready text.

| PRD section | Pull into Stitch |
|-------------|------------------|
| Problem / goal | Opening line of `designMd` + each screen prompt (“Purpose: …”) |
| User stories / acceptance criteria | Bullet constraints in `generate_screen_from_text` prompt (“Must show…”, “When X, then Y”) |
| Flows / steps | Ordered list of screens to generate; prototype order in Stitch |
| Edge cases / errors | Separate generation or explicit paragraph in prompt |
| Non-goals | “Do not include …” in `designMd` and prompts |
| Copy / legal | Exact strings if PRD mandates; otherwise paraphrase |

## designMd skeleton (paste into create_design_system theme)

Keep under ~8k chars if the API limits; summarize aggressively.

```markdown
# Design context (from PRD)

## Source
- PRD: [URL or "pasted"]
- Feature: [name]

## Goals
- [bullets from PRD]

## Users & context
- [persona / JTBD]

## Screens to build (priority order)
1. [Screen A — key requirements]
2. [Screen B — …]

## Global UI rules
- Platform: mobile web (mWeb) unless PRD says otherwise
- Accessibility: [from PRD or WCAG AA default]
- Out of scope: [from PRD]

## Per-screen notes
### Screen A
- [bullets]

### Screen B
- [bullets]
```

## Stitch MCP folder

Tool schemas live under the Cursor project MCP config, e.g.:

`mcps/user-stitch/tools/create_project.json`  
`mcps/user-stitch/tools/create_design_system.json`  
`mcps/user-stitch/tools/update_design_system.json`  
`mcps/user-stitch/tools/generate_screen_from_text.json`  
`mcps/user-stitch/tools/get_screen.json`  
`mcps/user-stitch/tools/list_projects.json`

Always **open the JSON** for the tool you are about to call; enums and required fields change.

## generate_screen_from_text — prompt pattern

```
PRD-grounded UX for [feature], screen: [name].

Source requirements (from PRD):
- [acceptance criteria / bullets]

Layout (mWeb / mobile web): single column, touch targets, [specific components].
Visual: [if using redbus STITCH.md tokens, name them: color.brand.primary, etc.]
Do not include: [non-goals from PRD].
```

## PRD link types

| Type | How to ingest |
|------|----------------|
| Google Doc | May need user to set sharing or **File → Download → paste** |
| Notion | Public page fetch or export / paste |
| Confluence | Export or paste if auth blocks fetch |
| GitHub | `Read` the file path or raw URL |
| PDF URL | Fetch if public; else user uploads or pastes text |

If fetch returns login HTML or empty, **stop and ask** for paste or export.
