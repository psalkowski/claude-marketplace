# Execution modes — deciding how to run the work

Pick after the spec (and optional plan doc) is approved. Announce the choice in one sentence; the user can override.

## 1. Same-session, subagent-driven (default)

This session stays the orchestrator and dispatches per-task executors using the same policy table as `spec:execute` (authorship → `spec:plan-executor`, chores → `spec:plan-executor-light`, escalation → `spec:plan-executor-heavy`, review gate via `spec:plan-reviewer`).

**Why it's the default:** it splits the cost correctly. The planning context — explored files, key facts, grilling outcomes — is reused here at ~0.1× via prompt cache, while the expensive part, *code emission*, happens as executor output at the executor's price. Output tokens are never cached; the main-session model hand-writing implementations is the most expensive way to produce them.

No plan doc required — write each dispatch prompt as a self-contained brief: intent, files, key facts (with an exemplar — `file:line` of similar existing code — where one exists), constraints, acceptance criteria, targeted verification command, and the spec's path so the executor can self-serve.

## 2. Same-session, inline

Execute right here: derive the task list from the spec, enter plan mode, get approval, implement in this context.

**Choose when** the work is small enough that dispatch overhead exceeds the work itself: a handful of files, a few working turns, no parallelism to exploit. Trivial edits never justify a subagent round-trip.

**Keep it lean:** bulk file reading and test runs still go to subagents; don't pull whole files into this context when a throwaway one can read them.

## 3. Fresh-session handoff (`spec:execute`)

Requires a plan doc (Phase 5).

**Choose when** the work spans sessions or days, execution is deferred ("run it tonight"), or the current context is near its limit.

**Economics, for calibration:** the handoff costs plan-writing output plus full-price re-ingestion in the new session (typically 30–100k+ token-equivalents). Carrying this session's planning context costs ~0.1× of it per further turn. Crossover is around 15+ turns of remaining work — below that, stay in-session.

## Every mode

- **Verification policy:** targeted tests per task; full build + lint + suite **once** at the end, or when the user asks for a PR.
- Escalation-only routing; never silently downgrade work to a cheaper model.
- An under-briefed executor is the orchestrator's defect: if a task goes wrong for lack of context, fix the brief before re-dispatching.
