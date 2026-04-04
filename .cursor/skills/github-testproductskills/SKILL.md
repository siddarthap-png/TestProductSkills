---
name: github-testproductskills
description: >-
  Team workflow for siddarthap-png/TestProductSkills: clone, auth, PRs, README updates for new
  paths, commit messages with repo-purpose tag, anti-delete safeguards, overlapping skills, and
  versioning. Use for TestProductSkills, publishing .cursor/skills, or GitHub skill repo hygiene.
---

# GitHub team workflow: `siddarthap-png/TestProductSkills`

This skill is written so **any teammate** can **clone on their own computer**, **authenticate with their own GitHub login**, and **create/update content** via normal Git (and optionally Cursor). Nothing here assumes a single shared PC or a shared password.

## Canonical repository

- **Owner / repo**: `siddarthap-png` / `TestProductSkills`
- **HTTPS**: `https://github.com/siddarthap-png/TestProductSkills.git`
- **SSH**: `git@github.com:siddarthap-png/TestProductSkills.git`
- **Web**: `https://github.com/siddarthap-png/TestProductSkills`

Use this repo when the task is to add, update, or edit **skills** (or other files) there, unless the user names another clone, fork, or repo.

### Canonical repository purpose (reuse in commits)

Use this **exact short line** whenever you append the repo purpose to a commit (see **Commit messages** below):

**`TestProductSkills — shared Cursor agent skills for product & engineering`**

(Optional longer context for PR bodies only: skills for GitHub repo workflow, JIRA↔Excel MCP sync, GA4 events from Figma / Redbus taxonomy, etc.)

## README.md (required when adding new paths)

The repo root **`README.md`** is the **map** for humans browsing on GitHub. Keep it **accurate** and **short**.

| Rule | Detail |
|------|--------|
| **New folder or top-level file** | Whenever you add a **new** directory under the repo (e.g. a new `.cursor/skills/<new-skill>/`, or a new top-level folder like `docs/`), **update `README.md` in the same commit** (or the same PR). |
| **One line per entry** | Add a **single concise row** to the layout table (or bullet list): **path** + **what it is for** (one line, no essays). |
| **Skills** | Each `.cursor/skills/<name>/` gets one row describing **trigger / audience** (match the skill’s `description` frontmatter, shortened). |
| **No stale rows** | If a path is removed with explicit approval, remove or mark deprecated in README in that same change set. |

Suggested **`README.md` shape:** one short intro paragraph, then a **Repository layout** table (`Path` | `Description`), then a **Contributing** line pointing at this skill for Git + safety rules.

## Circulating this skill to the team

Each person needs the **skill instructions** on their machine (so Cursor/agents can follow the same rules):

| Approach | What to do |
|----------|------------|
| **Copy the skill folder** | Copy the directory `github-testproductskills/` (containing `SKILL.md`) into their project’s `.cursor/skills/` or personal `~/.cursor/skills/` (see [Cursor skills locations](https://cursor.com/docs)). |
| **Pull from Git** | Clone or pull `TestProductSkills`; point Cursor at that workspace, or copy `.cursor/skills/github-testproductskills/SKILL.md` into their own repo’s `.cursor/skills/`. |
| **Single source of truth** | After this file is updated on `main`, teammates should **pull** their clone or recopy `SKILL.md` so everyone runs the same version. |

**Do not** commit **team-wide** secrets into the repo. Each developer uses **their own** tokens/keys on **their own** OS user profile.

## Access: who can push what

| Situation | What each member needs |
|-----------|-------------------------|
| **Direct collaborator** | GitHub account added with **Write** (or **Maintain/Admin**) on `siddarthap-png/TestProductSkills`. They clone the **upstream** URL and push branches or open PRs depending on team rules. |
| **No write access yet** | Fork the repo to **their** GitHub user, clone **their fork**, add `upstream` remote to the org repo, work on a branch, **push to their fork**, open a **PR** to `siddarthap-png/TestProductSkills`. |
| **Read-only** | Clone and branch locally for review; cannot push until access is granted or a fork PR is used. |

The agent should **never** assume credentials from another person’s machine; the **logged-in user** must be able to `git push` successfully from **their** terminal after one-time setup.

## First-time setup on your machine (each teammate)

1. **Install Git** — [Git for Windows](https://git-scm.com/download/win), or Xcode CLT / `git` on macOS, or distro package on Linux.
2. **Set your commit identity** (used on every commit; should be **your** name/email):
   - `git config --global user.name "Your Name"`
   - `git config --global user.email "your@email"` (prefer the email tied to your GitHub account, or GitHub noreply address from profile settings).
3. **Choose authentication** for GitHub HTTPS or SSH (see **Connection and authentication** below). Complete **one** successful `git fetch` or `git push` from a normal system terminal so credentials are stored.
4. **Clone the repo** to a folder **you** own (examples):
   - Windows: `C:\Users\<you>\src\TestProductSkills` or `...\Documents\GitHub\TestProductSkills`
   - macOS/Linux: `~/src/TestProductSkills` or `~/Projects/TestProductSkills`
   - Command:  
     `git clone https://github.com/siddarthap-png/TestProductSkills.git <path-you-pick>`
5. **Open that folder in Cursor** when editing skills so paths and Git context match the clone.

If you use a **fork**, clone **your** fork instead and set `upstream`:

```bash
git clone https://github.com/<your-username>/TestProductSkills.git
cd TestProductSkills
git remote add upstream https://github.com/siddarthap-png/TestProductSkills.git
git fetch upstream
```

## Preferred workflow: local git (team-friendly)

1. **Work in your clone** — Use the folder from step 4 above (not someone else’s path).
2. **Stay current** — `git fetch origin` (and `git fetch upstream` if using a fork), merge or rebase `main` before large edits when the team agrees.
3. **Branch** — `git checkout -b skills/<short-topic>` or `feature/<name>` (avoid pushing straight to `main` if the team uses PR review).
4. **Edit** — Change files under `.cursor/skills/` (and elsewhere) following repo layout and [skill naming](#skill-naming-this-repo’s-skills).
5. **README** — If you created a **new** folder or meaningful top-level path, **edit `README.md`** per **[README.md (required when adding new paths)](#readmemd-required-when-adding-new-paths)** before committing.
6. **Commit** — `git add` / `git commit` with a message that follows **[Commit messages (repo purpose)](#commit-messages-repo-purpose)** and **Versioning and history**.
7. **Push** — `git push -u origin <branch>` (origin = your fork or the org repo, depending on setup).
8. **Integrate** — Open a **Pull Request** on GitHub for review and merge to `main` when required.

**Fork workflow:** push branch to **your fork’s** `origin`, open PR **from your fork → `siddarthap-png/TestProductSkills:main`**.

**Before every commit (deletion safety):** run `git status` and `git diff --stat` (or `git diff --cached --stat` after staging). If anything appears under **deleted** or you see unexpected removals in the diff, **stop** and fix the working tree before committing—do not push accidental deletes.

## No accidental file deletion

Goal: **no skill or repo file disappears by mistake**—not from a bad `git add`, not from an agent “cleanup,” not from a rushed merge. Covers **human process**, **GitHub settings**, and **agent rules**.

### Humans (every commit and PR)

| Rule | Detail |
|------|--------|
| **Explicit removes only** | Delete or `git rm` a file **only** when the task clearly requires it (e.g. documented deprecation, user said “remove X”). Never delete to “tidy” unless the team agreed. |
| **Inspect before commit** | After staging: `git diff --cached --name-status`. Any line starting with **`D`** (deleted) must be **intentional**. If not, `git restore --staged <path>` and `git restore <path>` to recover. |
| **Prefer narrow staging** | Use `git add path/to/file` or `git add .cursor/skills/specific-folder/` instead of blind `git add -A` at repo root when unrelated files might be missing locally. |
| **PR review** | Every change to `main` goes through a **Pull Request**. Reviewers must scan the **Files changed** tab for **red** deletions; reject or question any removal that was not discussed. |
| **Recovery** | If a file was deleted only locally: `git restore <path>` or `git checkout HEAD -- <path>`. If already pushed: restore from a previous commit on GitHub (**History** → view file at old commit) or `git revert` the bad commit. |

### GitHub repository settings (repo admin — strongly recommended)

Configure on **`siddarthap-png/TestProductSkills`** → **Settings**:

| Setting | Purpose |
|---------|---------|
| **Branch protection rules** for `main` | Require a **pull request** before merging; require **approval** from at least one reviewer (or CODEOWNERS). Optionally **dismiss stale reviews** when new commits are pushed. |
| **Block force pushes** to `main` | Prevents history rewrite that could hide or obliterate files without a clear trail. |
| **Restrict who can push** | Limit direct pushes to `main` to admins only (everyone else uses PRs). |

Optional: add **[CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)** for `.cursor/skills/**` so deletion/rename PRs notify owners.

### Agents and automation (mandatory)

- **Do not** run `git rm`, **do not** delete files or skill folders, and **do not** use the GitHub API **DELETE** on repo contents **unless the user explicitly asked to remove that specific path** (by name) and the impact was restated for confirmation.
- **Do not** use “cleanup,” “remove duplicates,” or “consolidate” as a reason to delete without **written user confirmation** that lists the exact paths to remove.
- Before suggesting `git commit`, remind the user to verify **`git status`** and that **no unintended deletions** are staged.
- If a PR would delete files, **call out every deleted path** in the summary so humans can catch mistakes.

**Note:** Branch protection and review reduce risk; they do not replace careful `git status` / diff review. Admins with bypass rights must follow the same rules.

## Overlapping skills: state the difference before merge or replace

When a **new or updated skill** targets the **same end goal** as an existing skill under `.cursor/skills/<name>/SKILL.md` (similar description, triggers, or workflow):

1. **Inventory** — List every existing skill whose purpose could collide (read `name`, YAML `description`, and section **Scope** / **Objective** if present).
2. **Explicit comparison** — In the PR description, commit message body, or a short `## Change summary` comment for the user, document a **difference table** (or bullet list) covering at minimum:
   - **Audience / when to use** (triggers the agent should prefer one vs the other)
   - **Scope boundaries** (what one skill covers that the other does not)
   - **Outcomes** (deliverables, tools, repos, or MCP servers involved)
   - **Deprecation** — If one skill **supersedes** another, say so and name the folder to remove or archive; do not leave two skills with an unexplained duplicate purpose.
3. **Choose one path**
   - **Single skill**: Merge content into one `SKILL.md`. **Removing** a redundant skill folder is allowed **only** under **[No accidental file deletion](#no-accidental-file-deletion)**—user must explicitly approve the paths to delete, PR must list deletions, reviewers must confirm.
   - **Separate skills**: Prefer **rename or rewrite** descriptions so each skill’s **unique** objective is obvious; avoid two files that read like the same instructions—**avoid delete** unless explicitly approved.
4. **Use git to compare** — Run `git diff main -- .cursor/skills/` (or diff against the prior commit for that path) so the change is reviewable as text, not only as a blob replace.

Never **silently overwrite** a skill on `main` when another submission matches its purpose unless the comparison above is recorded in **git history** (commit message or PR).

## Commit messages (repo purpose)

Every commit that touches this repository should make the **repository’s purpose** obvious in GitHub’s history (for people who work across many repos).

**Append** the canonical purpose line to the commit as follows (pick one style; be consistent as a team):

1. **Subject suffix (preferred when it fits ~72 characters total)**  
   - Format: `<type>(<scope>): <short change> | TestProductSkills — shared Cursor agent skills for product & engineering`  
   - Example: `skills(ga-events-from-figma): add Peru DNI examples | TestProductSkills — shared Cursor agent skills for product & engineering`

2. **Subject + body (when the subject would be too long)**  
   - **Subject:** `<type>(<scope>): <short change>`  
   - **Body:** final non-empty line must be exactly:  
     `TestProductSkills — shared Cursor agent skills for product & engineering`

**Types** (examples): `skills`, `docs`, `chore` — scope is often the skill folder name or `readme`.

**Agents:** When proposing a commit message, **always** include the purpose line (suffix or body footer) and **mention if `README.md` was updated** when new paths were added.

## Versioning and history (required before merging or publishing)

Git is the **source of truth** for skill history. Keep it **inspectable** and **ordered**.

| Practice | Why |
|----------|-----|
| **One logical commit per skill change** when possible | `git log --follow -- .cursor/skills/<name>/SKILL.md` stays readable. |
| **Conventional, specific commit subjects** | e.g. `skills(jira-sheet-sync): clarify MCP apply step | TestProductSkills — …` — not `update skill`. |
| **Body in commit message** for substantive edits | What changed, why, overlap resolution; **end** with repo purpose line if not in subject (see **Commit messages (repo purpose)**). |
| **README in sync** | New paths documented in `README.md` in the same PR/commit when possible. |
| **Branches + PRs to `main`** | GitHub PR thread + merge commit preserve **review history** and intent. |
| **Optional frontmatter `version:`** | e.g. SemVer `1.0.0` on `SKILL.md`; bump **minor** for added behavior, **patch** for clarifications, **major** for breaking workflow changes. Increment in the same commit as the content change. |
| **Optional repo `CHANGELOG.md`** | For releases across many skills; newest entry first, link to paths under `.cursor/skills/`. |
| **Git tags (optional)** | e.g. `skills-2026-04-04` or per-skill tag only if the team uses releases — not mandatory for every edit. |

**How history is accessed**

- **Local:** `git log --oneline -- .cursor/skills/` and `git log -p --follow -- path/to/SKILL.md`
- **GitHub:** file view → **History**; compare branches or SHAs in the UI; PR **Files changed** for the full narrative.

**Do not** use the GitHub Contents API to **replace** a skill file in production without a corresponding **git-style** record of what differed (prefer clone → commit → push so history is linear and searchable). API is still acceptable for one-off automation if the user accepts weaker audit trail.

## Connection and authentication (your login, your machine)

Git operations must run as **the same OS user** who completed GitHub login. Cursor’s integrated terminal can be **non-interactive**; you may see:

- `fatal: could not read Username for 'https://github.com': No such file or directory`
- `error: failed to execute prompt script` / `/dev/tty: No such file or device`

**Fix in order of reliability:**

1. **Git Credential Manager (GCM)** — Bundled with **Git for Windows**. Ensure:
   - `git config --global credential.helper manager` (on Windows; on macOS/Linux use platform-recommended helper or `gh` below).
   - Run **one** successful `git push` or `git fetch` from **outside** Cursor (system Terminal / PowerShell / cmd) so a browser or dialog can store **your** GitHub credentials.

2. **GitHub CLI** (`gh`) — Works on [Windows, macOS, Linux](https://cli.github.com/).
   - Windows: `winget install --id GitHub.cli -e --source winget`
   - After install, **`gh auth login`** then **`gh auth setup-git`** (not `gh setup-git`).
   - If `gh` is not found, restart the terminal or use the full path (Windows: `C:\Program Files\GitHub CLI\gh.exe`).

3. **Personal access token (HTTPS)** — GitHub → **Settings → Developer settings** → PAT (classic) with **`repo`**, or a **fine-grained** token with **Contents** read/write on `TestProductSkills`. When Git prompts for a password over HTTPS, paste the **token**, not your GitHub password.

4. **SSH** — Generate a key on **your** machine, add the **public** key to **your** GitHub account, then:
   - `ssh -T git@github.com` (should greet **you** by username).
   - `git remote set-url origin git@github.com:siddarthap-png/TestProductSkills.git` (or your fork URL).

5. **Non-interactive / agent runs** — Do not assume prompts work. Prefer GCM/`gh` already logged in; optional **`GH_TOKEN`** / **`GITHUB_TOKEN`** only for tools that support it—**never** commit tokens to the repo.

6. **If push still fails** — Report the exact error; complete auth in a **system** terminal with the same repo, then retry in Cursor.

**Default:** **HTTPS + GCM** (Windows) or **HTTPS + `gh auth setup-git`**, unless the team standardizes on SSH.

## Alternative: GitHub Contents API (single-file, no local clone)

Use only when a local clone is impractical and the change is a **single file** (**create or update only**).

- **Endpoint**: `PUT https://api.github.com/repos/siddarthap-png/TestProductSkills/contents/{path}`
- Requires a token with **`contents: write`** for that repo (the **user** or **automation account** that owns the token must have access).
- For updates, `GET` the file first for `sha`, then `PUT` with Base64 content per GitHub API docs.
- `Authorization: Bearer <token>` — **never** commit tokens.
- **Do not** use the API **DELETE** endpoint for repo files in this workflow unless the user **explicitly** requested deletion of that path and understands it is permanent on the default branch after merge.

Prefer local git for multi-file changes, refactors, and team review.

## Agent checklist

- [ ] Repo target is `siddarthap-png/TestProductSkills` (or the user’s **fork** of it), not a similarly named repo.
- [ ] **No accidental deletion:** do not delete files or run `git rm` unless the user **explicitly** named paths to remove; before any commit suggestion, flag any **staged or proposed deletions** for human review (see **No accidental file deletion**).
- [ ] Use the **user’s local clone path** they opened in Cursor—do not hardcode another teammate’s directory.
- [ ] Confirm **push** will use **their** credentials (they have completed GCM/`gh`/SSH setup).
- [ ] If the change **adds or alters** a skill that **overlaps** another: document differences (see **Overlapping skills**) in the commit message or PR.
- [ ] **README:** if any **new** folder or top-level path was added, **`README.md`** lists it with a **short** description in the same change set.
- [ ] **Commit message** includes the **TestProductSkills** purpose line (suffix or body footer) per **Commit messages (repo purpose)**.
- [ ] **Versioning / history**: meaningful commit subject/body; optional `version:` bump on material changes; prefer **branch + PR** when multiple skills change.
- [ ] Working tree is clean enough to commit; no unrelated files staged; **`git diff --cached --name-status`** has no surprise **`D`** rows.
- [ ] User has **push** access or is using **fork → PR**; if not, explain what access they need from the repo owner.
- [ ] On auth errors, follow **Connection and authentication**; do not loop failing `git push`.
- [ ] After push, summarize branch, commit message, PR link, and where to see **history** (local `git log` or GitHub **History**).

## Skill naming (this repo’s skills)

Workspace convention: **folder name = frontmatter `name`**, kebab-case, short and specific (e.g. `github-testproductskills`, `jira-sheet-sync`). One skill per folder; entry file is always **`SKILL.md`**. If the consuming project has a rule file (e.g. **cursor-ai-reference-root**), follow that schema for new skills.
