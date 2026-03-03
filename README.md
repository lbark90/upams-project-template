# UPAMS Project Template

Use this template when creating a new project in the UPAMS automated pipeline.

## Included

- `.github/copilot-instructions.md` — Copilot coding agent + review guardrails
- `.github/workflows/auto-pr-on-push.yml` — Auto-opens a PR on every push to non-main branches
- `CLAUDE.md` — Claude Code session instructions (update project name, repo, project ID, stack)
- `.mcp.json` — MCP config for UPAMS persistent storage (update PROJECT_STATE_PROJECT_ID)

## After creating from this template

1. Register in UPAMS registry
2. Update CLAUDE.md and .mcp.json with project-specific values
3. Enable Copilot auto-review ruleset in repo settings
4. Verify webhook exists pointing to UPAMS orchestrator

See full SOP: https://github.com/lbark90/upams/blob/main/docs/new-project-sop.md
