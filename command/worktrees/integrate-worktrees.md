---
description: Integrate winning worktree and clean up
argument-hint: [review-recommendation]
agent: worktree-integrator
subtask: true
---

# Integrate Worktrees Command

Integrate the winning implementation (merge/cherry-pick for git, copy guidance for folders) and safely clean up.

## Usage

```bash
# After /review-worktrees, integrate the winner
/integrate-worktrees

# Or paste the review recommendation
/integrate-worktrees <paste review output>

# Or manually specify what to merge
/integrate-worktrees --merge ai/JIRA-123/claude-sonnet-4-5

# Or cherry-pick specific commits
/integrate-worktrees --cherry-pick <sha1>,<sha2>,<sha3>
```

## What It Does

**Git Mode:**
1. **Validates state** - ensures working tree is clean
2. **Integrates changes** - merge or cherry-pick as recommended
3. **Runs validation** - executes tests if available
4. **Cleans up worktrees** - safely removes worktrees and branches
5. **Reports final state** - shows what was integrated

**Folder Mode:**
1. **Verifies workspaces** - confirms winner exists
2. **Provides copy guidance** - exact commands to use winner
3. **Suggests cleanup** - manual commands to remove other folders (user runs when ready)
4. **No automatic integration** - user has full control

## Safety

**Git Mode:**
- Checks for uncommitted changes before cleanup
- Never force-removes worktrees
- Provides rollback commands if integration fails
- Stops on conflicts for manual resolution

**Folder Mode:**
- No automatic deletion (provides cleanup commands for user)
- User keeps full control over when to delete folders
- Can archive implementations for reference instead of deleting

## Options

- `--merge <branch>` - Full merge of winning branch
- `--cherry-pick <shas>` - Selective commit cherry-picking
- `--keep-worktrees` - Don't delete worktrees after integration
- `--keep-branches` - Don't delete branches after integration

---

**You provided:** ${input}

Integrating winning solution and cleaning up worktrees...
