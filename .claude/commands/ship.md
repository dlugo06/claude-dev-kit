Ship the current branch: simplify, test, commit, push, create PR, and launch reviewers.

Optional argument: PR title override (e.g., `/ship fix: handle edge case`)

## Steps

### 1. Validate Branch

```bash
git branch --show-current
```

If on `master` — STOP. Tell the user to create a feature branch first. Never ship from master.

### 2. Check Working State

```bash
git status
git diff --stat
gh pr list --head <branch-name> --json number,url,state
git log origin/<branch-name>..HEAD --oneline 2>/dev/null
```

Determine what work has ALREADY been done:
- PR already open? → skip PR creation in step 8, use existing PR number
- Already pushed with no new local commits? → skip push in step 7
- No uncommitted changes? → skip commit in step 5 (but still run simplify + tests)

If no changes (staged or unstaged), no unpushed commits, AND no existing PR — STOP. Nothing to ship.

### 3. Simplify Changed Code

Run `/simplify` on files that were modified in this branch (compared to master):

```bash
git diff master --name-only
```

Apply any improvements suggested by simplify. If simplify changes code, re-run tests after.

### 4. Run Tests

```bash
pytest
```

ALL tests must pass. If any fail — STOP. Fix failures before shipping.

### 5. Commit

If there are uncommitted changes:

- Stage modified files (not untracked files unless they're clearly part of the work)
- Write a conventional commit message (`feat:`, `fix:`, `test:`, `refactor:`, `docs:`)
- Commit

If changes were already committed, skip this step.

### 6. Spec Check

Launch the spec-checker agent **in the foreground** (must complete before continuing):

- If the user provided a spec path → pass it explicitly
- Otherwise → let the agent auto-detect from branch name

**Agent** `subagent_type: "spec-checker"` with prompt: `"Run spec check"` (or `"Check spec at <path>"` if known).

**If verdict is PASS** → continue to step 7.

**If verdict is FAIL** → STOP. Show the user the blocking issues from the agent output, then ask:

> "Spec check failed. How would you like to proceed?
> 1. Fix the issues and re-run /ship
> 2. Continue anyway (skip spec compliance)
> 3. The spec is outdated — skip spec check"

Wait for the user's response before continuing. Do NOT proceed to push or PR creation until resolved.

**If no spec found for the branch** → STOP. Ask the user which spec to use:

> "No spec found matching branch `<branch-name>`. Available specs in `docs/superpowers/specs/`:"
> (list all specs)
> "Which spec should I check against, or type 'skip' to skip the spec check?"

Wait for the user's response before continuing.

### 7. Push

```bash
git push -u origin <branch-name>
```

### 8. Create PR

```bash
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<1-3 bullet points summarizing ALL commits on this branch>

## Test plan
- [ ] All tests pass
- [ ] Coverage meets 70%+ threshold

## TDD compliance
- [ ] Tests written before implementation
- [ ] RED → GREEN → REFACTOR cycle followed
EOF
)"
```

If the user provided a title argument, use it. Otherwise, generate one from the branch name and commit messages. Keep it under 70 characters.

If a PR already exists for this branch, skip creation and use the existing PR number.

### 9. Launch Reviewers

Launch all 3 in a SINGLE message (parallel tool calls, all in background):

1. **Agent** `subagent_type: "pr-reviewer"` — custom 5-dimension project review, posts to GitHub
2. **Agent** `subagent_type: "pr-review-toolkit:silent-failure-hunter"` — error handling analysis
3. **Agent** `subagent_type: "security-reviewer"` — security assessment (OWASP scan, secret handling, input validation), posts to GitHub

Pass the PR number to all 3 agents.

### 10. Report

After reviewers complete, output a summary:

```
## Ship Complete

- Branch: <branch>
- PR: <url>
- Commit(s): <count> commits pushed
- Tests: <count> passed, <coverage>% coverage
- Reviews: 3 launched (pr-reviewer, silent-failure-hunter, security-reviewer)
```
