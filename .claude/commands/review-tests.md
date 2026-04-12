Audit test quality for the current branch before shipping.

Launch the **test-quality-reviewer** agent to cross-reference tests against the approved scenario plan, flag weak assertions, check edge case coverage, and report a PASS/NEEDS WORK verdict.

## Steps

1. Launch the `test-quality-reviewer` agent (subagent_type: "test-quality-reviewer") in the foreground
2. The agent will read all changed test and source files, compare against `.dev/test-plan-<branch>.md`, and write findings to `.dev/test-review-<branch>.md`
3. Present the verdict to the user:
   - **PASS** → "Test quality review passed. Ready to `/ship`."
   - **NEEDS WORK** → Show the blocker list and ask: "Fix these issues before shipping, or override with `/ship` to skip."
