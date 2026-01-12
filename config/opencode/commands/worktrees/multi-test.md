---
description: Run multiple agents in parallel worktrees to compare implementations
argument-hint: <task-description-or-jira-key> [--agents agent1,agent2,agent3]
agent: worktree-orchestrator
subtask: true
---

# Multi-Agent Testing Command

Run multiple frontier model agents in parallel to compare their implementations of the same task.

Works in **both modes**:
- **Git repos**: Uses `git worktree` for isolated branches
- **Non-git/greenfield**: Uses separate folders for each agent

## Usage

```bash
# With default agents (claude-sonnet-4-5, gpt-5-2, gemini-3-pro)
/multi-test "Implement user authentication with JWT tokens"

# With Jira ticket
/multi-test JIRA-9322

# With specific agents
/multi-test JIRA-9322 --agents gemini-3-pro,qwen3-coder,claude-sonnet-4-5

# With number of agents (picks best N)
/multi-test "Add pagination to user list API" --num-agents 5
```

## What It Does

1. **Creates workspaces** for each agent:
   - **Git mode**: Creates worktrees at `../kato-<agent-name>/` with branches `ai/<task>/<agent>`
   - **Folder mode**: Creates directories at `../<project>-<agent-name>/`
2. **Spawns subagents** in parallel to implement the task independently
3. **Collects results** and produces a structured comparison report
4. **Hands off to reviewer** for scoring and integration recommendation

## Agent Selection

**Default agents** (if none specified):
- `claude-sonnet-4-5` - Claude Code Max subscription
- `gpt-5-2` - ChatGPT Pro subscription  
- `gemini-3-pro` - Gemini Pro subscription

**Available agents:**
- All frontier models configured in `~/.config/opencode/agent/`
- Quality-first: prefers subscription-backed over paid API calls
- Falls back gracefully if a model is unavailable

## Output

The command produces:
- Orchestration report showing what each agent changed
- Ready for review with `/review-worktrees` command
- Then integration with `/integrate-worktrees` command

## Mode Detection

The orchestrator automatically detects:
- **Git repo**: Uses worktrees with branches
- **Non-git directory**: Uses folders without version control

Both modes are fully supported and produce comparable results.

## Safety

- Never pushes to remote (git mode)
- Creates isolated workspaces (worktrees OR folders)
- Git mode: Creates branches `ai/<task-slug>/<agent-name>`
- Non-destructive (keeps workspaces until you explicitly integrate/clean up)
- Can be aborted at any time

## Example Sessions

### Git Repo (e.g., Kato)

```bash
# Start multi-agent test
/multi-test JIRA-9322 --agents claude-sonnet-4-5,gpt-5-2,o3

# ... agents work in background ...

# Review results
/review-worktrees

# ... see scores and recommendation ...

# Integrate winner
/integrate-worktrees
```

### Non-Git / Greenfield Project

```bash
# In an empty directory or non-git project
/multi-test "Build a React app with user authentication"

# ... agents create separate implementations ...

# Review results
/review-worktrees

# ... see scores and recommendation ...

# Choose winner and clean up
/integrate-worktrees
```

---

**You provided:** ${input}

**Parsing arguments...**

Let me parse your input and kick off the multi-agent orchestration:

1. **Task identification**
   - If starts with project key pattern (e.g., JIRA-123, ABC-456): treat as Jira ticket
   - Otherwise: treat as free-form task description

2. **Agent selection**
   - Look for `--agents` flag with comma-separated list
   - Look for `--num-agents` flag with number
   - Default to 3 frontier models if neither provided

3. **Invoke orchestrator**
   - Pass parsed task and agent list to `worktree-orchestrator` agent
   - Orchestrator creates worktrees and spawns subagents

Starting orchestration now...
