Run interactive E2E tests against the live application using Playwright MCP.

Usage: `/e2e-test <phase|task_id> [task_id...]`

Examples:
- `/e2e-test phase2a` — test all tasks in Phase 2a
- `/e2e-test P2a-007` — test a specific task
- `/e2e-test P2a-007 P2a-010` — test multiple tasks

## Steps

1. Parse the arguments. They will be a phase identifier (e.g., `phase2a`) or one or more task IDs (e.g., `P2a-007`). If no arguments, error with usage instructions.

2. Launch the `e2e-tester` agent (subagent_type: "e2e-tester") with prompt: "Run E2E verification for: {args}. Follow your full procedure: open the application → read PRD → execute steps via Playwright MCP → write report → update PRD → close browser."

3. When the agent returns, summarize results to the user:
   - How many tasks tested, steps passed/failed
   - Path to the full report (`.dev/E2E_REPORT_{phase}.md`)
   - Any tasks that had their `passes` field updated
