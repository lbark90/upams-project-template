# Claude Code Instructions — <Project Name>

## Session Start (REQUIRED)

At the beginning of every session, load project context from UPAMS before doing anything else:

```
mcp__upams__get_project_status()
```

This gives you active tasks, recent decisions, open issues, and codebase patterns accumulated from prior sessions. Use this knowledge instead of re-exploring the codebase cold — it directly reduces token usage.

If the user mentions a specific topic, also run:
```
mcp__upams__semantic_search(query="<topic>", collection="all")
```

## During Work

**Before exploring files you have not read**, search MCP first.

**Save as you go — every significant action:**
- Starting a new task → `mcp__upams__manage_tasks(action="add", title="...", status="in_progress")`
- Chose an architecture, library, or pattern → `mcp__upams__add_decision()`
- Found a bug, gotcha, or file-level pattern → `mcp__upams__add_note(category="finding")`
- Finished a task → `mcp__upams__manage_tasks(action="complete", task_id="...")`
- Completed a feature or shipped something → `mcp__upams__add_milestone()`

**Cadence rule:** If you have taken 5+ meaningful actions without saving to MCP, stop and save before continuing.

## Session End (REQUIRED — do not skip)

1. Complete open tasks: `mcp__upams__manage_tasks(action="complete", task_id="...")`
2. Save session summary: `mcp__upams__add_note(content="Session summary: ...", category="research")`
3. Save new patterns: `mcp__upams__add_note(content="...", category="finding")`
4. Check token budget if long session: `mcp__upams__check_token_budget(current_tokens=<estimate>)`

## New Project Onboarding (One-Time SOP)

When first setting up a new repo in the UPAMS pipeline, Claude must do the following automatically — do NOT wait to be asked:

1. **Read the codebase tech stack** — check `package.json`, `next.config.*`, `vite.config.*`, or other root config files to identify the framework, ORM, UI library, and auth approach.
2. **Update `.github/copilot-instructions.md`** — replace or create it with content that accurately reflects this repo's actual stack. Use the standard pipeline context block, then add a "Tech Stack" section listing only what this repo actually uses.
3. **Add or update `.github/dependabot.yml`** — use the standard config (weekly, group minor+patch for both dev and production deps, block major bumps).
4. **Add or update `CLAUDE.md`** — fill in the Project Context section with the actual repo name, MCP project ID, and stack.
5. Commit each file as a separate `chore:` commit, or bundle them into one `chore: UPAMS pipeline setup`.

This ensures every new project has correct, repo-specific Copilot instructions without any manual copy-paste.

## Project Context

- **Repo**: `lbark90/<repo-name>`
- **MCP project ID**: `<project-id>`
- **Stack**: [e.g. Express + React + Vite + TypeScript]
- **UPAMS pipeline**: Issues labeled `upams` → Claude/Copilot writes fix → Copilot reviews → Gemini approves → auto-merge

## Branch Strategy

- `main` — **production branch**. Deploys to Cloud Run on every merge. Never push directly.
- All other branches — development. Push to a branch, a PR is auto-opened, reviewed, and merged into `main`.
- `v-1`, `develop`, `feature/*` etc. — dev branches. PRs from these auto-target `main`.
- Agent branches (`claude/*`, `copilot/*`) — created automatically by Claude/Copilot agents. Do not push to these manually.

## Key Rules

- Always check MCP for prior context before reading files you have not touched this session
- Always save non-obvious decisions to MCP so the next session does not repeat the reasoning
- Keep notes specific and actionable
- **Never commit directly to `main`** — always go through a PR
