# START HERE: Your New AI Config Setup

Welcome! This file explains your newly restructured AI configuration for dual-tool development.

## TL;DR - What Changed

**Before**: Single `opencode-config/` with mixed agent definitions

**After**: 
- `claude/` → symlinked to `~/.claude/` for Claude Code (Kato work)
- `opencode/` → symlinked to `~/.config/opencode/` for OpenCode (personal projects)
- Clear separation: work agents vs. personal agents
- Language-specific agents: rust-ai-engineer, python-ai-researcher, gamedev-haxe

**Why**: 
- Single source of truth (repo) for both tools
- Easy syncing across machines via git
- Context-aware agent loading (backend code → backend-laravel, Rust code → rust-ai-engineer)
- Cost-optimized defaults (Haiku 4.5 for work, GLM-4.7 FREE for personal)

## Quick Setup (5 minutes)

```bash
# 1. Create Claude symlink
ln -sf /home/mrmg/Development/Build/opencode-config/claude ~/.claude

# 2. Create OpenCode symlink
mkdir -p ~/.config
ln -sf /home/mrmg/Development/Build/opencode-config/opencode ~/.config/opencode

# 3. Verify both exist
ls ~/.claude/agents.yaml
ls ~/.config/opencode/agents.json

# 4. Test Claude Code (open Kato, should see agents)
# 5. Test OpenCode (open personal Rust project, should see rust-ai-engineer)
```

Done! No copying, no duplicates, no syncing headaches.

## What You Have

### Claude Code (Kato Work)
- **Primary**: Sisyphus (Haiku 4.5) - Fast, cost-efficient, 73.3% SWE-bench
- **Specialists**: backend-laravel, frontend-react-ts, test-engineer, security-auditor, database-architect
- **Premium**: Architect-Opus (only for complex migrations)
- **Cost**: ~$40/month (95% savings vs. naive Sonnet/Opus)
- **Auto-detection**: backend/ → backend-laravel, frontend/ → frontend-react-ts

### OpenCode (Personal)
- **Primary**: Sisyfreeus (GLM-4.7 FREE) - 73.8% SWE-bench, 100% FREE
- **Specialists**: rust-ai-engineer, python-ai-researcher, gamedev-haxe
- **Research**: librarian-free, oracle-free (Gemini Pro for complex reasoning)
- **Cost**: FREE for most work, ~$5-10/month for premium reasoning
- **Auto-detection**: rust-* → rust-ai-engineer, python-* → python-ai-researcher, game-* → gamedev-haxe

## Directory Structure

```
opencode-config/
├── claude/                    ← Symlink to ~/.claude/ (Claude Code)
│   ├── agents.yaml            Kato agents
│   ├── workflows.yaml         Kato workflows
│   ├── workspace.yaml         Kato context routing
│   └── README.md              Claude docs
│
├── opencode/                  ← Symlink to ~/.config/opencode/ (OpenCode)
│   ├── agents.json            Personal agents
│   ├── workflows.yaml         Personal workflows
│   ├── spaces.yaml            Workspace detection
│   ├── config.jsonc           Plugins/MCP
│   ├── commands/              Custom commands
│   └── README.md              OpenCode docs
│
├── README.md                  ← Project overview
├── MIGRATION_GUIDE.md         ← Detailed setup
├── RESTRUCTURE_PLAN.md        ← Full architecture
├── IMPLEMENTATION_CHECKLIST.md ← Full checklist
├── SETUP_SUMMARY.txt          ← Visual summary
└── 00_START_HERE.md           ← This file!
```

## Reading Order

1. **This file** (you're reading it!) - Overview
2. **SETUP_SUMMARY.txt** - Visual quick reference
3. **MIGRATION_GUIDE.md** - Detailed setup for each machine
4. **claude/README.md** - Claude Code details
5. **opencode/README.md** - OpenCode details
6. **RESTRUCTURE_PLAN.md** - Full architecture (if curious)
7. **IMPLEMENTATION_CHECKLIST.md** - Comprehensive planning

## How It Works

### Editing Config

```bash
# Edit Claude agents
vim /home/mrmg/Development/Build/opencode-config/claude/agents.yaml
# Changes live immediately (symlink reads from repo)

# Edit OpenCode agents
vim /home/mrmg/Development/Build/opencode-config/opencode/agents.json
# Changes live immediately

# Commit and sync
cd /home/mrmg/Development/Build/opencode-config
git add . && git commit -m "Update agents" && git push
```

### On Other Machines

```bash
cd /home/mrmg/Development/Build/opencode-config
git pull
# Changes are live immediately (symlinks already point to repo!)
# No need to restart tools or recreate symlinks
```

## Expected Behavior

### Claude Code

Open Kato repository:
```
Kato/
├── backend/app/Services/UserService.php
│   → Claude auto-detects "backend/" path
│   → Loads: Sisyphus + backend-laravel + test-engineer
│   → Workflow: monorepo-backend (Eloquent, Laravel conventions)
│
└── frontend/src/components/Dashboard.tsx
    → Claude auto-detects "frontend/" path
    → Loads: Sisyphus + frontend-react-ts + frontend-ui-ux-engineer
    → Workflow: monorepo-frontend (React hooks, TypeScript strict)
```

### OpenCode

Open personal project:
```
~/Development/Personal/rust-algorithms/
→ OpenCode auto-detects "rust-*" path pattern
→ Loads: Sisyfreeus + rust-ai-engineer
→ Workflow: rust-ai (ownership, async, trait design)

~/Development/Personal/python-ml/
→ OpenCode auto-detects "python-*" path pattern
→ Loads: Sisyfreeus + python-ai-researcher
→ Workflow: python-ai (PyTorch, NumPy, vectorization)

~/Projects/2d-game/
→ OpenCode auto-detects "game-*" path pattern
→ Loads: Sisyfreeus + gamedev-haxe
→ Workflow: gamedev-haxe (Heaps.io, performance, visuals)
```

## Cost Summary

| Tool | Baseline | Specialists | Premium | Monthly |
|------|----------|-------------|---------|---------|
| Claude (Kato) | Haiku 4.5 ($1/$5) | Gemini Flash ($0.075/$0.30) | Opus 4.5 ($5/$25) | ~$40 |
| OpenCode (Personal) | GLM-4.7 (FREE) | Gemini Flash ($0.30) | Gemini Pro ($1.05) | FREE + $5-10 opt |
| **Total** | | | | **~$45-50/mo** |

Previous naive approach: ~$800/month
With your optimization: ~$50/month
**Savings: 94% cost reduction!**

## Common Tasks

### Task: Fix a bug in Kato backend
```
1. Open backend/app/Services/UserService.php in Claude Code
2. Claude auto-loads backend-laravel agent
3. Ask Claude to fix the bug
4. backend-laravel knows: Eloquent patterns, Laravel conventions, PHPUnit testing
```

### Task: Implement Rust async feature
```
1. Open Rust project in OpenCode (e.g., ~/Development/Personal/rust-algorithms/)
2. OpenCode auto-loads rust-ai-engineer agent
3. Ask for async/await pattern
4. rust-ai-engineer knows: ownership, trait design, tokio, async patterns
```

### Task: Compare Python vs Rust for algorithm
```
1. In OpenCode, write Python version in ~/Development/Personal/python-ml/
2. Use /multi-test "Implement this algorithm in Rust"
3. python-ai-researcher + rust-ai-engineer both implement it
4. Use /review-worktrees to score both approaches
5. Use /integrate-worktrees to merge the better one
```

## Troubleshooting

**Claude doesn't see agents?**
→ Verify `ls ~/.claude/agents.yaml` exists
→ Restart Claude Code
→ See MIGRATION_GUIDE.md for details

**OpenCode doesn't auto-load agents?**
→ Check path patterns in `~/.config/opencode/spaces.yaml` match your projects
→ Verify `ls ~/.config/opencode/agents.json` exists
→ Restart OpenCode
→ See MIGRATION_GUIDE.md for details

**Symlinks broken?**
→ On Windows, use directory junctions: `mklink /J` (see MIGRATION_GUIDE.md)
→ On Linux/Mac, verify with `ls -la ~/.claude` and `ls -la ~/.config/opencode`

## What's Next

1. **Follow SETUP_SUMMARY.txt** (5-10 minute setup)
2. **Test Claude Code** with Kato repo
3. **Test OpenCode** with personal project
4. **Push to git** to back up your config
5. **Pull on other machines** and verify sync

That's it! You're done.

## Questions?

- **Setup issues?** → See MIGRATION_GUIDE.md
- **Agent details?** → See claude/README.md or opencode/README.md
- **Full architecture?** → See RESTRUCTURE_PLAN.md
- **Comprehensive checklist?** → See IMPLEMENTATION_CHECKLIST.md

---

**Version**: 1.0 (Jan 2026)
**Status**: Ready to deploy
**Last Updated**: Just now

Let's get started! 🚀
