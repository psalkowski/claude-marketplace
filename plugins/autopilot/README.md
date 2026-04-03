# Autopilot

Fully autonomous issue-to-PR workflow. Pass a GitHub issue, get a complete PR with no human intervention.

## How it Works

Autopilot runs 9 sequential phases, each with specialized agents working in parallel:

1. **Discovery** — Fetch issue, classify type, gather requirements (3 parallel agents)
2. **Brainstorm** — Agents ask and answer each other's clarifying questions (max 3 rounds)
3. **Design** — Parallel design specs based on issue classification (2-5 agents)
4. **Design Exploration** — HTML playground creation and browser-based visual evaluation
5. **Implementation** — Parallel frontend, backend, and database developers
6. **Testing** — Unit, integration, E2E, and accessibility tests
7. **Manual QA** — Real browser testing via Playwright MCP tools
8. **Review** — Spec compliance, code quality, and convention enforcement
9. **Ship** — Commit, push, and create PR with comprehensive summary

## Agents

| Agent | Role |
| --- | --- |
| issue-analyst | Parses and classifies GitHub issues |
| product-owner | Produces requirements documents from issue classifications |
| domain-expert | Validates requirements against business rules |
| solutions-architect | Designs architecture and produces task lists |
| database-architect | Designs database schemas and migration strategies |
| ui-designer | Designs UI/UX including component trees and responsive behavior |
| api-designer | Designs API contracts and endpoint specifications |
| security-analyst | Identifies security risks and produces constraints |
| devils-advocate | Challenges designs and flags unresolved concerns |
| frontend-developer | Implements frontend code |
| backend-developer | Implements backend code |
| database-developer | Creates migrations and repository functions |
| integration-developer | Handles cross-service wiring |
| unit-test-writer | Writes unit tests for UI components and hooks |
| integration-test-writer | Writes API integration tests |
| e2e-test-writer | Writes Playwright E2E tests |
| accessibility-tester | Verifies WCAG AA compliance |
| spec-compliance-verifier | Verifies implementation matches requirements |
| code-quality-reviewer | Reviews code quality and maintainability |
| convention-enforcer | Runs project verification and enforces conventions |
| design-playground-builder | Creates interactive HTML design playgrounds |
| design-explorer | Explores design playgrounds via browser to select best designs |
| manual-qa-tester | Tests the app in a real browser via Playwright |

## Usage

```
/autopilot <issue-url-or-number>
```

Examples:

```
/autopilot https://github.com/org/repo/issues/42
/autopilot 42
```

## Requirements

- `gh` CLI installed and authenticated
- `git` remote configured

## Integration

Project-specific conventions — quality gates, commit rules, linting requirements, testing standards — are defined in the host project's `CLAUDE.md` and applied automatically by the agents. Autopilot reads this context before starting work, so it adapts to any project without configuration.
