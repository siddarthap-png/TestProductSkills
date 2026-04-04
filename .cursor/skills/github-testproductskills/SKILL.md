---
name: github-testproductskills
description: >-
  Adds, updates, or edits files in the GitHub repository siddarthap-png/TestProductSkills.
  Enforces comparing overlapping skills, documenting differences before merge, and preserving
  git history and versioning. Use when the user mentions TestProductSkills, skill updates to
  GitHub, merging skills, versioning, or push/clone for that repo.
---

# GitHub: siddarthap-png/TestProductSkills

## Repository

- **Owner / repo**: `siddarthap-png` / `TestProductSkills`
- **HTTPS clone URL**: `https://github.com/siddarthap-png/TestProductSkills.git`
- **SSH URL** (optional): `git@github.com:siddarthap-png/TestProductSkills.git`
- **Web**: `https://github.com/siddarthap-png/TestProductSkills`

Treat this as the **canonical target** whenever the user asks to add, update, or edit files in that repository (unless they explicitly point at another clone or fork).

## Recommended clone location (this workspace)

Prefer a stable working copy under the user’s Cursor reference tree:

`C:\Users\siddartha.p\Desktop\CursorProject\GitHub\TestProductSkills`

Use that path when cloning if the workspace is not already that repo.

## Preferred workflow: local git

1. **Ensure a working copy**
   - If the current workspace **is** `TestProductSkills` (or a worktree of it), work there.
   - Otherwise clone once (or open an existing clone), then use that folder as the workspace for edits:
     - `git clone https://github.com/siddarthap-png/TestProductSkills.git "C:\Users\siddartha.p\Desktop\CursorProject\GitHub\TestProductSkills"`
   - Confirm `git remote -v` shows `origin` → `siddarthap-png/TestProductSkills` (or the user's fork if they use fork workflow).

2. **Branch**
   - Create or use a feature branch: `git checkout -b <short-descriptive-name>` (avoid committing directly to `main` unless the user insists).

3. **Edit files**
   - Add new files or modify existing ones with normal editor/tools; match existing project layout, naming, and style.

4. **Commit**
   - Stage and commit with a clear message:
     - `git add ...` then `git commit -m "..."`
   - One logical change per commit when possible.

5. **Push and integrate**
   - `git push -u origin <branch>`
   - Open a PR on GitHub unless the user wants a direct push to `main`.

6. **Auth (see next section)** — set this up **before** relying on automated or agent-driven pushes.

## Overlapping skills: state the difference before merge or replace

When a **new or updated skill** targets the **same end goal** as an existing skill under `.cursor/skills/<name>/SKILL.md` (similar description, triggers, or workflow):

1. **Inventory** — List every existing skill whose purpose could collide (read `name`, YAML `description`, and section **Scope** / **Objective** if present).
2. **Explicit comparison** — In the PR description, commit message body, or a short `## Change summary` comment for the user, document a **difference table** (or bullet list) covering at minimum:
   - **Audience / when to use** (triggers the agent should prefer one vs the other)
   - **Scope boundaries** (what one skill covers that the other does not)
   - **Outcomes** (deliverables, tools, repos, or MCP servers involved)
   - **Deprecation** — If one skill **supersedes** another, say so and name the folder to remove or archive; do not leave two skills with an unexplained duplicate purpose.
3. **Choose one path**
   - **Single skill**: Merge content into one `SKILL.md`, delete or redirect the redundant folder in the **same** change set, and explain the consolidation in the commit message.
   - **Separate skills**: Rename or rewrite **descriptions** so each skill’s **unique** objective is obvious; avoid two files that read like the same instructions.
4. **Use git to compare** — Run `git diff main -- .cursor/skills/` (or diff against the prior commit for that path) so the change is reviewable as text, not only as a blob replace.

Never **silently overwrite** a skill on `main` when another submission matches its purpose unless the comparison above is recorded in **git history** (commit message or PR).

## Versioning and history (required before merging or publishing)

Git is the **source of truth** for skill history. Keep it **inspectable** and **ordered**.

| Practice | Why |
|----------|-----|
| **One logical commit per skill change** when possible | `git log --follow -- .cursor/skills/<name>/SKILL.md` stays readable. |
| **Conventional, specific commit subjects** | e.g. `skills(jira-sheet-sync): clarify MCP apply step` — not `update skill`. |
| **Body in commit message** for substantive edits | What changed, why, and if overlap was resolved (see previous section). |
| **Branches + PRs to `main`** | GitHub PR thread + merge commit preserve **review history** and intent. |
| **Optional frontmatter `version:`** | e.g. SemVer `1.0.0` on `SKILL.md`; bump **minor** for added behavior, **patch** for clarifications, **major** for breaking workflow changes. Increment in the same commit as the content change. |
| **Optional repo `CHANGELOG.md`** | For releases across many skills; newest entry first, link to paths under `.cursor/skills/`. |
| **Git tags (optional)** | e.g. `skills-2026-04-04` or per-skill tag only if the team uses releases — not mandatory for every edit. |

**How history is accessed**

- **Local:** `git log --oneline -- .cursor/skills/` and `git log -p --follow -- path/to/SKILL.md`
- **GitHub:** file view → **History**; compare branches or SHAs in the UI; PR **Files changed** for the full narrative.

**Do not** use the GitHub Contents API to **replace** a skill file in production without a corresponding **git-style** record of what differed (prefer clone → commit → push so history is linear and searchable). API is still acceptable for one-off automation if the user accepts weaker audit trail.

## Connection and authentication (Windows / Cursor)

Cursor’s integrated terminal and some automation contexts are **non-interactive**. Git may fail with messages like:

- `fatal: could not read Username for 'https://github.com': No such file or directory`
- `error: failed to execute prompt script` / `/dev/tty: No such file or device` (credential helper cannot show a prompt)

**Fix in order of reliability:**

1. **Git Credential Manager (GCM)** — included with [Git for Windows](https://git-scm.com/download/win). Ensure it is active:
   - `git config --global credential.helper manager`
   - Perform **one** successful `git push` or `git fetch` from **Windows Terminal** or **Command Prompt** (outside Cursor) so GCM can open a browser or dialog and store credentials.

2. **GitHub CLI** (`gh`)
   - **Install** (Windows): `winget install --id GitHub.cli -e --source winget` — or download from [cli.github.com](https://cli.github.com/).
   - If PowerShell says `gh` is not recognized, **close and reopen the terminal** (or sign out/in) so `Path` picks up `C:\Program Files\GitHub CLI\`. You can also run: `& "C:\Program Files\GitHub CLI\gh.exe" version`
   - **Authenticate and wire Git:** run **`gh auth login`** first, then **`gh auth setup-git`** (not `gh setup-git` — the `auth` part is required).
   - After that, HTTPS Git operations use the CLI-backed credential flow.

3. **Personal access token (HTTPS)**
   - GitHub → Settings → Developer settings → PAT (classic) with **`repo`** scope, or fine-grained token with **Contents** read/write on `TestProductSkills`.
   - When Git asks for a password over HTTPS, paste the **token**, not the GitHub account password.

4. **Non-interactive / agent runs**
   - Do **not** assume an interactive prompt is available.
   - Options: pre-authenticate using steps 1–3, or set environment variable **`GH_TOKEN`** or **`GITHUB_TOKEN`** (with `repo` access) for tools that honor it; for Git itself, GCM-stored credentials are still preferred after a one-time login outside Cursor.

5. **SSH**
   - Verify: `ssh -T git@github.com` (should greet you by username).
   - If `git push` **hangs**, common causes: missing `ssh-agent` / key not loaded, corporate proxy, or first-time host key prompt in a context that cannot display it. Run `ssh -T` once in an interactive terminal to accept `github.com` if needed.
   - Remote: `git remote set-url origin git@github.com:siddarthap-png/TestProductSkills.git`

6. **If push still fails**
   - Tell the user exactly which error appeared; suggest completing authentication in **Windows Terminal** with the same repo, then retry in Cursor.

**Default recommendation:** Use **HTTPS + GCM** (or **HTTPS + `gh auth setup-git`**) unless the user already standardized on SSH.

## Alternative: GitHub Contents API (single-file, no local clone)

Use only when a local clone is impractical and the change is a **single file** (create or update).

- **Create / update file**: `PUT https://api.github.com/repos/siddarthap-png/TestProductSkills/contents/{path}`
- Requires a token with `contents: write` (fine-grained: Contents read and write on that repo).
- For updates, first `GET` the same path to obtain the file `sha`, then include that `sha` in the PUT body.
- Base64-encode file content per GitHub API docs.
- Supply the token via `Authorization: Bearer <token>` header (never commit tokens).

Prefer local git for multi-file changes, refactors, and anything that needs tests or formatting.

## Agent checklist

- [ ] Target is explicitly `siddarthap-png/TestProductSkills` (not a similarly named repo).
- [ ] If the change **adds or alters** a skill that **overlaps** another skill’s purpose: **difference / consolidation** is documented (see **Overlapping skills**) in the commit message or PR before merge.
- [ ] **Versioning / history**: meaningful commit message; bump optional `version:` in `SKILL.md` when behavior or scope changes materially; prefer **branch + PR** to `main` over silent direct edits when multiple skills are touched.
- [ ] Working tree is clean enough to commit; no unrelated files staged.
- [ ] User has push access (or fork + PR if they use forks).
- [ ] If a **connection** or **authentication** error occurs, follow **Connection and authentication** above; do not loop on failing `git push` without fixing credentials.
- [ ] After push, summarize branch name, commit message, PR link if created, and **how to view history** (`git log` path or GitHub **History** on the file).

## Skill naming (this repo’s skills)

Workspace convention: **folder name = frontmatter `name`**, kebab-case, short and specific (e.g. `github-testproductskills`, `jira-sheet-sync`). One skill per folder; entry file is always **`SKILL.md`**. See project rule **cursor-ai-reference-root** for the full schema.
