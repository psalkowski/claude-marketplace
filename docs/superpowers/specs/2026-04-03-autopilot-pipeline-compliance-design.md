# Autopilot Pipeline Compliance Fix

## Problem

The autopilot skill has two failure modes that cause Claude to skip phases and go straight to ad-hoc implementation:

1. **Worktree crash**: Phase 1.1 unconditionally requires `EnterWorktree`. Projects that forbid worktrees in their CLAUDE.md cause an unresolvable instruction conflict. Claude abandons the structured pipeline entirely.

2. **Phase skipping**: The skill is 718 lines of prose with no enforcement mechanism. Claude loses focus and skips phases — particularly attachment analysis, bug reproduction, manual QA, and review.

## Solution

### Change 1: Mandatory TaskCreate Checklist

Add a two-stage checklist system that forces Claude to acknowledge every phase:

**Stage 1 — Bootstrap (before Phase 0):**
- A mandatory instruction at the top of SKILL.md: create a task for "Phase 0: Ticket Analysis" before doing anything else
- This hooks Claude into the task tracking system from the very first action

**Stage 2 — Full checklist (new Phase 0.5, after Phase 0 gate):**
- After ticket analysis completes and ISSUE_CLASSIFICATION is known, create tasks for all remaining phases
- Tasks are tailored to the ticket type:
  - Always: Phase 1 (Discovery), Phase 2 (Brainstorm), Phase 3 (Design), Phase 4 (Implementation), Phase 5 (Testing), Phase 6 (Manual QA), Phase 7 (Review), Phase 8 (Ship)
  - Conditional: Phase 1.5 (Bug Reproduction) — only for bugs with user-facing symptoms
  - Conditional: Phase 3.5 (Design Exploration) — only when areas includes "frontend"
- Each task must be marked `in_progress` when the phase starts and `completed` when its gate passes

### Change 2: Optional Worktree

Make worktree creation conditional:

- Before attempting `EnterWorktree`, read the project's CLAUDE.md for worktree prohibitions
- If forbidden: create a branch with `git checkout -b claude/autopilot-$SOURCE_ID`
- If `EnterWorktree` fails for any other reason: fall back to branch mode
- Store `WORKSPACE_MODE` ("worktree" or "branch") for use in Phase 8.5 cleanup
- Update error recovery table: worktree failure = fallback, not abort
- Update 4 agent files that reference "worktree" to use neutral wording ("on the isolated branch")

## Files to Modify

| File | Change |
|------|--------|
| `plugins/autopilot/skills/autopilot/SKILL.md` | Add bootstrap checklist (top), add Phase 0.5, modify Phase 1.1, update error recovery, update Phase 8.5 |
| `plugins/autopilot/agents/frontend-developer.md` | Line 15: remove "worktree" reference |
| `plugins/autopilot/agents/backend-developer.md` | Line 15: remove "worktree" reference |
| `plugins/autopilot/agents/database-developer.md` | Line 15: remove "worktree" reference |
| `plugins/autopilot/agents/integration-developer.md` | Line 15: remove "worktree" reference |
| `plugins/autopilot/.claude-plugin/plugin.json` | Version bump |
| `.claude-plugin/marketplace.json` | Version bump |

## Verification

1. Read the modified SKILL.md and confirm the bootstrap checklist is the first instruction
2. Confirm Phase 0.5 creates tasks for all phases after ticket analysis
3. Confirm Phase 1.1 has worktree detection + fallback logic
4. Confirm error recovery table no longer says "Abort" for worktree failure
5. Confirm all 4 agent files no longer reference "worktree"
6. Confirm versions match in plugin.json and marketplace.json
