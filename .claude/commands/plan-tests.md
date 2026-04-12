Generate test scenario plans for the current branch's implementation plan.

Launch the **test-scenario-planner** agent to analyze the implementation plan, read source code, and produce a structured Given/When/Then scenario map per task. Run this after `writing-plans` and before `executing-plans` or `subagent-driven-development`.

## Steps

1. Launch the `test-scenario-planner` agent (subagent_type: "test-scenario-planner") in the foreground
2. The agent will read the plan, source code, and `.ai/test-scenarios-guide.md`, then write scenarios to `.dev/test-plan-<branch>.md`
3. Present the scenario plan to the user for review and approval
4. If the user requests changes, relay them to the agent and regenerate
5. Once approved, confirm: "Test scenario plan approved. Ready for implementation with `/execute` or `subagent-driven-development`."
