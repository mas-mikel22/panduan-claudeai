# Copilot / AI Agent Instructions — Claude Code CLI Repo

Short actionable guidance for AI coding agents working in this repository.

1. Purpose & big picture
   - This repo documents installation and usage of the Claude Code CLI and VS Code extension (no app source code). See `README.md` for the user-facing guide and `TROUBLESHOOTING.md` for common failures.
   - Core integrations: the CLI/extension communicate with the Anthropic API at `https://api.z.ai/api/anthropic` using `ANTHROPIC_AUTH_TOKEN` and `ANTHROPIC_BASE_URL` environment variables.

2. Quick dev workflow & verification steps
   - Verify prerequisites: Node.js >= 18 (check `node --version`).
   - Validate CLI install: `npm install -g @anthropic-ai/claude-code` then `claude --version`.
   - For extension testing: prefer workspace settings (`.vscode/settings.json`) using `claudeCode.environmentVariables` as documented in `README.md`.

3. Important commands and CLI behavior to reference in suggestions
   - `claude` — launches the CLI (note: the command is `claude`, not `claude-code`).
   - In-CLI commands: `/status`, `/cost`, `/help`, `/model`, `/exit`.
   - `/cost` shows token usage (input + output). There is no command to return a prompt count; use `/cost` and `PENJELASAN_RATE_LIMIT.md` notes to estimate limits.

4. Windows-specific conventions & gotchas (very important)
   - Environment variables (Windows): `setx ANTHROPIC_AUTH_TOKEN your_api_key` and `setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic` (restart terminal/VS Code after `setx`).
   - Common problems documented in `TROUBLESHOOTING.md`: PATH not updated for npm globals (`%APPDATA%\npm`), PowerShell execution policy blocking `npm.ps1`, and when to use CMD vs PowerShell (use CMD if execution policy prevents npm).
   - If `claude` not found: confirm `npm config get prefix` and ensure that location (usually `%APPDATA%\npm`) is on PATH.

5. Rate limits and monitoring
   - Concurrency limits: models impose concurrent-request limits (e.g., 10). Excess concurrent requests cause `429` or "Rate limit exceeded".
   - Plan limits: the repo documents a 120-prompts-per-5-hours plan (see `PENJELASAN_RATE_LIMIT.md`). Use `/cost` token totals to estimate prompt counts (document contains heuristics).

6. Security & repo conventions
   - Never suggest committing API keys; follow `README.md` security section: use environment vars or workspace settings.
   - Prefer workspace `.vscode/settings.json` for extension secrets (more reliable on Windows).

7. What to edit / where to look when asked
   - User-facing documentation: `README.md` (installation + usage), `TROUBLESHOOTING.md` (errors & fixes), `PENJELASAN_RATE_LIMIT.md` (limits + monitoring).
   - Additions should preserve the user-oriented guidance and troubleshooting steps already present.

8. Examples of specific, safe actions you may suggest or take in PRs
   - Add a short troubleshooting step to `TROUBLESHOOTING.md` for any recurring Windows PATH or execution-policy issue you observe in user reports.
   - Add a small example workspace `.vscode/settings.json.example` demonstrating `claudeCode.environmentVariables` (do not include real API tokens).
   - If adding scripts, keep them simple and platform-aware (e.g., separate PowerShell and bash snippets) and update `README.md` accordingly.

9. What *not* to assume
   - There is no service or server code here to run or test — this repo documents install + usage. Don’t invent backend code or endpoints beyond the documented Anthropic base URL.
   - Do not assume a CI or test matrix exists; check files before suggesting test scaffolding.

10. When you finish a change
    - Run your edits by the user. Ask: "Does this capture the Windows-specific troubleshooting and the environment-variable guidance you want emphasized?" and iterate.

References: `README.md`, `TROUBLESHOOTING.md`, `PENJELASAN_RATE_LIMIT.md`.

---

If you'd like, I can (1) tweak wording to be more concise, (2) add a short example `settings.json.example`, or (3) open a PR with these changes — which would you prefer?