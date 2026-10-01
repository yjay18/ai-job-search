---
framework_version: 1.0.0
---

# Agent Guidelines: AI Job Search

This workspace is structured to manage job search activities, scraper tools, CVs, cover letters, and interview preparation.

## Thin-Pointer Design (Single Source of Truth)

To prevent duplication and configuration drift across different AI agent frameworks (Claude Code, Google Antigravity, Codex, Cursor, Gemini CLI, etc.), this workspace uses a unified thin-pointer design. All agent runtimes should load the canonical specifications and candidate profiles from the files and directories below:

1. **Personal Candidate Profile:**
   - The candidate profile, contact details, education, and target preferences are defined in [CLAUDE.md](CLAUDE.md) and the individual profile methodology files under [.claude/skills/job-application-assistant/](.claude/skills/job-application-assistant/) (specifically `01-*.md` etc.).
2. **Canonical Workflow Specifications:**
   - The step-by-step instructions and triggers for tasks (setup, scrape, rank, apply, upskill, interview) are defined in the [.claude/](.claude/) directory (specifically under `.claude/skills/` and `.claude/commands/`).
   - Do not duplicate these rules or specifications. Treat `.claude/` files as the single source of truth.
3. **Portal Search Skills:**
   - Job-portal search CLIs live under [.agents/skills/](.agents/skills/) in the portable Agent Skills format (with a `SKILL.md` per portal). Codex and Antigravity discover these automatically; the `/scrape` workflow in [.claude/skills/job-scraper/](.claude/skills/job-scraper/) orchestrates them.

## Code Review Rules

For Codex and any reviewer of a pull request here. Report each finding with
the file and line, what is wrong, and the smallest fix; skip what a formatter
would catch.

- Repeated code: flag logic, constants or markup that repeat something already
  in the repository, even under another name, and point to the existing one.
- Shared modules: when the same helper now lives in two places, ask for it to
  live in one shared module and be imported from both.
- Generated filler: flag abstractions with one user, wrappers that only
  forward, defensive checks for states that cannot happen, comments that
  restate the code, unused imports, exports or parameters, and stub or
  placeholder code presented as finished.
- Documentation: when behaviour, configuration, commands or public interfaces
  change, check that the README, docs and code comments describing them change
  in the same pull request; flag anything stale or contradicting.
- Tests: where the repository has tests, new logic should come with a test
  that fails without it.
