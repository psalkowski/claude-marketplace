# Autopilot v2.3.0

Fully autonomous task-to-PR workflow. Pass a GitHub issue, Jira ticket, or plain text prompt — get a complete PR with no human intervention.

## How it Works

Autopilot runs 11 sequential phases, each with specialized agents working in parallel:

1. **Ticket Analysis** — Fetch input, analyze attachments (including video transcription), explore codebase, assess sufficiency
2. **Discovery** — Classify issue, gather requirements, validate against business rules (3 parallel agents)
3. **Bug Reproduction** — For bugs: reproduce in a real browser, capture baseline evidence for before/after comparison
4. **Brainstorm** — Agents ask and answer each other's clarifying questions (max 3 rounds)
5. **Design** — Parallel design specs based on issue classification (2-5 agents)
6. **Design Exploration** — HTML playground creation and browser-based visual evaluation
7. **Implementation** — Parallel frontend, backend, and database developers
8. **Testing** — Unit, integration, E2E, and accessibility tests
9. **Manual QA** — Real browser testing via Playwright MCP tools with before/after comparison and cross-role verification
10. **Review** — Spec compliance, code quality, and convention enforcement
11. **Ship** — Commit, push, and create PR with comprehensive summary

## Agents

| Agent | Role |
| --- | --- |
| input-resolver | Normalizes any input (GitHub URL, Jira URL, prompt) into structured data |
| media-analyzer | Downloads and analyzes ticket attachments including video transcription |
| issue-analyst | Parses and classifies issues by type and affected areas |
| product-owner | Produces requirements documents from issue classifications |
| domain-expert | Validates requirements against business rules |
| solutions-architect | Designs architecture and produces task lists |
| database-architect | Designs database schemas and migration strategies |
| ui-designer | Designs UI/UX including component trees and responsive behavior |
| api-designer | Designs API contracts and endpoint specifications |
| security-analyst | Identifies security risks and produces constraints |
| devils-advocate | Challenges designs and flags unresolved concerns |
| design-playground-builder | Creates interactive HTML design playgrounds |
| design-explorer | Explores design playgrounds via browser to select best designs |
| frontend-developer | Implements frontend code |
| backend-developer | Implements backend code |
| database-developer | Creates migrations and repository functions |
| integration-developer | Handles cross-service wiring |
| unit-test-writer | Writes unit tests for UI components and hooks |
| integration-test-writer | Writes API integration tests |
| e2e-test-writer | Writes Playwright E2E tests |
| accessibility-tester | Verifies WCAG AA compliance |
| bug-reproducer | Reproduces bugs in a real browser and captures baseline evidence |
| manual-qa-tester | Tests the app in a real browser with before/after comparison and cross-role verification |
| spec-compliance-verifier | Verifies implementation matches requirements |
| code-quality-reviewer | Reviews code quality and maintainability |
| convention-enforcer | Runs project verification and enforces conventions |

## Usage

```
/autopilot <issue-url-or-number-or-prompt>
```

Examples:

```
/autopilot https://github.com/org/repo/issues/42
/autopilot 42
/autopilot PROJ-456
/autopilot "Add dark mode toggle to the settings page"
```

## Requirements

- `gh` CLI installed and authenticated (for GitHub issues)
- `acli` installed and authenticated (for Jira tickets)
- `git` remote configured

## Integration

Project-specific conventions — quality gates, commit rules, linting requirements, testing standards — are defined in the host project's `CLAUDE.md` and applied automatically by the agents. Autopilot reads this context before starting work, so it adapts to any project without configuration.
