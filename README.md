# TestProductSkills

**TestProductSkills — shared Cursor agent skills for product & engineering.**  
This repo holds versioned [Cursor](https://cursor.com) **Agent Skills** under `.cursor/skills/` so teams can pull, copy, or reference them from GitHub.

## Repository layout

| Path | Description |
|------|-------------|
| `.cursor/skills/` | Root for all skills; one subfolder per skill. |
| `.cursor/skills/github-testproductskills/` | Git workflow for *this* repo: clone, auth, PRs, **README** updates for new paths, **commit message** repo-purpose line, safeguards against accidental deletes. |
| `.cursor/skills/jira-sheet-sync/` | Jira **Story** creation from Excel (`JIRA.xlsx`) via Jira MCP, REDRAIL acceptance criteria, `updates.json`, apply step. |
| `.cursor/skills/ga-events-from-figma/` | GA4 event payloads from Figma/UI specs using Redbus **`ep.`** / **`epn.`** taxonomy (load vs click, home/SRP/SL/cust info, Peru DNI addendum). |

## Contributing

- Use a **branch** and **Pull Request** to `main` (see branch protection if enabled).
- When you add a **new** folder or top-level path, add a **short** row to the table above in the **same PR**.
- Follow the **`github-testproductskills`** skill for commits (include the repo purpose line), overlap rules, and deletion safety.
