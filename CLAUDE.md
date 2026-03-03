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

**Before exploring files you haven't read**, search MCP first.

**Save as you go — every significant action:**
- Starting a new task → `mcp__upams__manage_tasks(action="add", title="...", status="in_progress")`
- Chose an architecture, library, or pattern → `mcp__upams__add_decision()`
- Found a bug, gotcha, or file-level pattern → `mcp__upams__add_note(category="finding")`
- Finished a task → `mcp__upams__manage_tasks(action="complete", task_id="...")`
- Completed a feature or shipped something → `mcp__upams__add_milestone()`

**Cadence rule:** If you've taken 5+ meaningful actions without saving to MCP, stop and save before continuing.

## Session End (REQUIRED — do not skip)

1. Complete open tasks: `mcp__upams__manage_tasks(action="complete", task_id="...")`
2. Save session summary: `mcp__upams__add_note(content="Session summary: ...", category="research")`
3. Save new patterns: `mcp__upams__add_note(content="...", category="finding")`
4. Check token budget if long session: `mcp__upams__check_token_budget(current_tokens=<estimate>)`

## Project Context

- **Repo**: `lbark90/<repo-name>`
- **MCP project ID**: `<project-id>`
- **Stack**: [e.g. Express + React + Vite + TypeScript]
- **UPAMS pipeline**: Issues labeled `upams` → Claude/Copilot writes fix → Copilot reviews → Gemini approves → auto-merge

## Key Rules

- Always check MCP for prior context before reading files you haven't touched this session
- Always save non-obvious decisions to MCP so the next session doesn't repeat the reasoning
- Keep notes specific and actionable
