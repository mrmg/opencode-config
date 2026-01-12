---
description: Review and score multi-agent worktree outputs
argument-hint: [orchestration-report]
agent: worktree-reviewer
subtask: true
---

# Review Worktrees Command

Score and compare the implementations from multiple agents working in parallel (git worktrees OR folders).

## Usage

```bash
# After running /multi-test, review the results
/review-worktrees

# Or paste the orchestration report
/review-worktrees <paste orchestration output>

# Or manually specify worktrees
/review-worktrees --worktrees ../kato-claude-sonnet-4-5,../kato-gpt-5-2,../kato-gemini-3-pro
```

## What It Does

1. **Inspects each workspace**:
   - **Git mode**: Compares diffs, commits, and changes
   - **Folder mode**: Compares structure, completeness, usability
2. **Scores agents** across categories (varies by mode):
   - **Git mode**: Correctness, Completeness, Code Quality, Scope Control, Safety/Risk, Test Readiness
   - **Folder mode**: Completeness, Usability, Code Quality, Production Readiness, Correctness
3. **Recommends integration strategy**:
   - **Git mode**: Merge, cherry-pick, or hybrid
   - **Folder mode**: Use winner, combine parts, or learn and rebuild

## Output

Produces a detailed review report with:
- Score table comparing all agents
- Per-agent strengths/weaknesses
- **Git mode**: Exact git commands for integration
- **Folder mode**: Copy/usage guidance and cleanup instructions
- Follow-up validation steps

## Next Step

After review, use `/integrate-worktrees` to merge the winning solution.

---

**You provided:** ${input}

Analyzing worktree outputs and scoring agents...
