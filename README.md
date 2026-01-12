# AI Configuration Repository

A unified configuration setup for dual-tool AI-assisted development: **Claude Code** (work) + **OpenCode** (personal).

## 🎯 Purpose

This repository contains configuration and agent definitions for:

- **Claude Code** - Production work on Kato (Laravel/React monorepo)
- **OpenCode** - Personal experimentation (Rust AI, Python ML, Haxe game dev)

Both tools are linked to directories in this repo using symlinks, enabling:
- Single source of truth for all agent definitions
- Automatic syncing across machines via git
- Context-aware agent selection based on file paths
- Consistent cost optimization across both tools

## 📁 Directory Structure

```
opencode-config/
├── config/                          # Configurations for symlinks
│   ├── claude/                      # → Symlink to ~/.claude
│   │   ├── agents.yaml
│   │   ├── workflows.yaml
│   │   ├── workspace.yaml
│   │   └── README.md
│   │
│   └── opencode/                    # → Symlink to ~/.config/opencode
│       ├── agents.json
│       ├── config.jsonc
│       ├── workflows.yaml
│       ├── spaces.yaml
│       ├── commands/
│       └── README.md
│
├── agent-reference/                 # Documentation (not symlinked)
│   ├── Sisyphus-haiku45.md
│   ├── Sisyfreeus.md
│   ├── Planner-Sisyfreeus.md
│   ├── Builder-Sisyfreeus.md
│   └── model-agents/
│
├── shared-concepts/                 # Documentation (not symlinked)
│   ├── workspace-detection.md
│   ├── flow-routing.md
│   └── cost-optimization.md
│
├── RESTRUCTURE_PLAN.md              # Architecture & rationale
├── MIGRATION_GUIDE.md               # Setup instructions
└── README.md                        # This file
```

## 🚀 Quick Start

### One-Time Setup (Per Machine)

```bash
# 1. Clone this repo (if not already done)
cd /home/mrmg/Development/Build
git clone <repo-url> opencode-config

# 2. Create symlinks
mkdir -p ~/.config
ln -sf /home/mrmg/Development/Build/opencode-config/config/claude ~/.claude
ln -sf /home/mrmg/Development/Build/opencode-config/config/opencode ~/.config/opencode

# 3. Verify both exist
ls -la ~/.claude/agents.yaml
ls -la ~/.config/opencode/agents.json
```

**Detailed setup**: See [MIGRATION_GUIDE.md](MIGRATION_GUIDE.md)

### Syncing Across Machines

```bash
# Make changes to any config
cd /home/mrmg/Development/Build/opencode-config
vim config/claude/agents.yaml
vim config/opencode/agents.json

# Commit and push
git add .
git commit -m "Update agents"
git push

# On other machines, just pull
git pull
# Symlinks already point to repo - changes are live!
```

## 🏗️ Architecture Overview

### Claude Code (Work)

**Kato Monorepo** - Full-stack Laravel/React production application

```
Claude Code
├── Primary Agent: Sisyphus (Haiku 4.5 - 73.3% SWE, $1/$5 per 1M)
├── Specialists: backend-laravel, frontend-react-ts, test-engineer
├── Workflows:
│   ├── monorepo-backend (Laravel development)
│   ├── monorepo-frontend (React/TypeScript development)
│   ├── api-contract (Backend ↔ Frontend integration)
│   └── security-audit (Security review)
└── Cost: ~$40/month (95% savings vs. Sonnet/Opus)
```

**Key Features**:
- Automatic agent loading based on file path (app/ → backend-laravel, ui/ → frontend-react-ts, resources/assets/js/angular/ → frontend-angularjs-legacy)
- Haiku 4.5 default for speed & cost (73.3% SWE-bench is excellent)
- Opus escalation path for complex migrations only
- Database architect, security auditor, test engineers for specialized work
- Mixed-stack support: Laravel backend + React modern frontend + AngularJS legacy frontend

### OpenCode (Personal)

**Multi-Language Experimentation** - Rust AI, Python ML, Haxe game dev

```
OpenCode
├── Primary Agent: Sisyfreeus (GLM-4.7 FREE - 73.8% SWE)
├── Language Specialists:
│   ├── rust-ai-engineer (ownership, async, performance)
│   ├── python-ai-researcher (PyTorch, JAX, algorithms)
│   └── gamedev-haxe (Heaps.io, visuals, performance)
├── Workflows:
│   ├── rust-ai (Rust algorithms & implementations)
│   ├── python-ai (ML research, experiments)
│   ├── gamedev-haxe (2D game development)
│   └── worktree-testing (Multi-agent parallel testing)
└── Cost: FREE (GLM-4.7, MiniMax M2.1, Grok Code + ~$5/month for Gemini Pro reasoning)
```

**Key Features**:
- Automatic workspace detection (Rust projects → rust-ai-engineer, Python projects → python-ai-researcher, etc.)
- All work uses FREE models (GLM-4.7, MiniMax M2.1, Grok Code)
- Advanced reasoning reserved for complex architectural decisions only
- Language-specific patterns and conventions built into agents

## 📊 Cost Optimization

### Claude Code (Kato Work)

| Model | Cost | Use | SWE-Bench |
|-------|------|-----|-----------|
| Haiku 4.5 (default) | $1/$5 per 1M | 90% of tasks | 73.3% |
| Gemini Flash | $0.075/$0.30 per 1M | Subagents (testing, security, DB) | 76% |
| Opus 4.5 | $5/$25 per 1M | Migrations, critical architecture | 80.9% |

**Strategy**: Default Haiku 4.5 for speed & cost. Escalate to Opus only for complex migrations or critical architectural decisions.

**Result**: ~$40/month vs. ~$780 with naive Sonnet/Opus throughout

### OpenCode (Personal Work)

| Model | Cost | Use | SWE-Bench |
|-------|------|-----|-----------|
| GLM-4.7 (default) | FREE | Primary, specialists, reasoning | 73.8% |
| MiniMax M2.1 | FREE | Fast implementation | 74% |
| Grok Code | FREE | Quick debugging | 70.8% |
| Gemini Flash | ~$0.30 per 1M | Research, exploration | 76% |
| Gemini Pro | ~$1.05 per 1M | Complex reasoning (reserve) | 76% |

**Strategy**: Default to FREE models for 90%+ of work. Reserve Gemini Pro only for complex reasoning.

**Result**: FREE for most personal work; ~$5-10/month for occasional premium reasoning

## 🎯 Key Features

### Workspace-Aware Agent Loading

Claude Code automatically detects context:
```
app/Models/User.php
→ Load: Sisyphus, backend-laravel, test-engineer
→ Workflow: monorepo-backend (Eloquent patterns, Laravel conventions)

ui/agency/App.tsx
→ Load: Sisyphus, frontend-react-ts, frontend-ui-ux-engineer
→ Workflow: monorepo-frontend (React hooks, TypeScript, Material-UI)

resources/assets/js/angular/agents-society/app.js
→ Load: Sisyphus, frontend-angularjs-legacy
→ Workflow: monorepo-frontend (AngularJS components, directives, UI Router)
```

OpenCode automatically detects workspace:
```
~/Development/Personal/rust-algorithms/
→ Load: Sisyfreeus, rust-ai-engineer
→ Workflow: rust-ai (ownership, trait design, async patterns)

~/Development/Personal/python-ml/
→ Load: Sisyfreeus, python-ai-researcher
→ Workflow: python-ai (PyTorch, NumPy, vectorization)

~/Projects/2d-game/
→ Load: Sisyfreeus, gamedev-haxe
→ Workflow: gamedev-haxe (Heaps.io, H3D, frame rate)
```

### Multi-Agent Testing (OpenCode)

```bash
/multi-test "Implement Rust async worker"
# Runs 3 agents in parallel, generates implementations
# Compares: correctness, style, performance

/review-worktrees
# Scores each implementation across categories

/integrate-worktrees
# Merges the best approach into main branch
```

### Unified Configuration

Single repo as source of truth:
- Edit agents → changes live in both tools (via symlinks)
- Commit → syncs across machines automatically
- No duplicate configs, no drift between machines

## 📖 Documentation

- **[MIGRATION_GUIDE.md](MIGRATION_GUIDE.md)** - Step-by-step setup for new machines
- **[RESTRUCTURE_PLAN.md](RESTRUCTURE_PLAN.md)** - Architecture, rationale, and design decisions
- **[config/claude/README.md](config/claude/README.md)** - Claude Code-specific documentation
- **[config/opencode/README.md](config/opencode/README.md)** - OpenCode-specific documentation
- **[agent-reference/](agent-reference/)** - Detailed agent personality docs
- **[shared-concepts/](shared-concepts/)** - Cost optimization, workspace detection, flows

## 🔄 Workflow Examples

### Claude Code: Fix a Backend Bug

```
1. Open Kato app/Models/User.php
2. Claude detects app/ → loads Sisyphus + backend-laravel
3. Ask: "Why is user creation failing in tests?"
4. backend-laravel knows Eloquent patterns, PHPUnit conventions
5. Provide fix with Laravel-specific understanding
```

### Claude Code: Update React Frontend

```
1. Open Kato ui/agency/App.tsx
2. Claude detects ui/ → loads Sisyphus + frontend-react-ts
3. Ask: "How do I add Redux state for this feature?"
4. frontend-react-ts knows Material-UI, Redux, React Router v5
5. Implement with proper patterns for agency app
```

### Claude Code: Maintain Legacy AngularJS

```
1. Open Kato resources/assets/js/angular/agents-society/app.js
2. Claude detects resources/assets/js/angular/ → loads Sisyphus + frontend-angularjs-legacy
3. Ask: "How do I update this UI Router state?"
4. frontend-angularjs-legacy knows AngularJS 1.5, directives, $q promises
5. Provide fix maintaining legacy patterns
```

### OpenCode: Optimize Rust Algorithm

```
1. Open ~/Development/Personal/rust-algorithms/src/main.rs
2. OpenCode detects rust-* → loads Sisyfreeus + rust-ai-engineer
3. Ask: "Can we make this async and use tokio?"
4. rust-ai-engineer understands ownership, trait design, tokio patterns
5. Implement with Rust best practices
6. /test-perf to compare before/after performance
```

### OpenCode: Compare Python vs Rust

```
1. Write Python algorithm in ~/Development/Personal/python-ml/
2. /multi-test "Rewrite this algorithm in Rust"
3. Runs both agents in parallel:
   - python-ai-researcher implements PyTorch version
   - rust-ai-engineer implements Tokio async version
4. /review-worktrees scores both by:
   - Correctness, readability, performance
5. /integrate-worktrees merges the better approach
```

## ✨ Status

- **Claude Setup**: ✅ Complete
  - agents.yaml with Sisyphus, Architect-Opus, specialists
  - workflows.yaml with monorepo-backend, monorepo-frontend, api-contract
  - workspace.yaml with context routing and conventions
  
- **OpenCode Setup**: ✅ Complete
  - agents.json with Sisyfreeus, language specialists, FREE models
  - workflows.yaml with rust-ai, python-ai, gamedev-haxe
  - spaces.yaml with workspace auto-detection
  - commands/worktrees/ for multi-agent testing

- **Documentation**: ✅ Complete
  - RESTRUCTURE_PLAN.md - full architecture
  - MIGRATION_GUIDE.md - setup instructions
  - claude/README.md, opencode/README.md - tool-specific docs
  - agent-reference/ - detailed personality docs

## 🚀 Next Steps

After setup:

1. **Test Claude Code**: Open Kato, verify agents load automatically
2. **Test OpenCode**: Open personal Rust project, verify rust-ai-engineer loads
3. **Test multi-agent**: Use `/multi-test` to compare implementations
4. **Commit a change**: Edit config, push, pull on another machine
5. **Iterate**: Add more language specialists or workflows as needed

## 📝 License & Notes

This is a personal configuration repository. Adapt to your needs!

Key principles:
- **Cost conscious**: Always ask "Is the extra cost justified?"
- **Task-specific**: Right tool for right job (Haiku vs. Opus, GLM vs. Gemini)
- **Documented**: Every agent and workflow has clear purpose
- **Maintainable**: Single source of truth, easy to update across machines

