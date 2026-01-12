# OpenCode Configuration for Personal Projects

This folder is symlinked to `~/.config/opencode/` (or `~/.opencode/`) on your personal machines.

## Files in This Directory

- **agents.json** - Agent definitions for OpenCode (personal projects only)
  - `Sisyfreeus` (GLM-4.7) - Primary FREE agent
  - `Planner-Sisyfreeus`, `Builder-Sisyfreeus` - Role variants
  - Language specialists:
    - `rust-ai-engineer` - Rust (ownership, async, performance)
    - `python-ai-researcher` - Python (PyTorch, JAX, algorithms)
    - `gamedev-haxe` - Haxe/Heaps (visual, performance)
  - Research agents: `oracle-free`, `reasoner-free`, `librarian-free`, `explore-free`
  - Utility agents: test-engineer-free, debugger-free, performance-optimizer-free, etc.

- **config.jsonc** - OpenCode plugins and MCP configuration
  - Chrome DevTools integration
  - Sequential thinking MCP
  - Atlassian integration (optional)

- **workflows.yaml** - Workflow definitions for personal projects
  - `rust-ai` - Rust AI experiments
  - `python-ai` - Python ML research
  - `gamedev-haxe` - Haxe/Heaps game dev
  - `worktree-testing` - Multi-agent parallel testing

- **spaces.yaml** - Workspace routing and auto-detection
  - Detects Rust projects → loads rust-ai-engineer
  - Detects Python projects → loads python-ai-researcher
  - Detects Haxe/game projects → loads gamedev-haxe
  - Detects general tinkering → loads Sisyfreeus

## Setup

Create symlink from this directory to OpenCode's config location:

```bash
mkdir -p ~/.config
ln -sf /path/to/opencode-config/opencode ~/.config/opencode

# OR if your system uses ~/.opencode:
ln -sf /path/to/opencode-config/opencode ~/.opencode
```

## Usage

### When you open a Rust project (e.g., `~/Development/Personal/rust-algorithms/`)
- **Auto-loaded agents**: `Sisyfreeus`, `rust-ai-engineer`
- **Workflow**: `rust-ai`
- **Context**: Ownership system, trait design, async patterns, tokio

### When you open a Python project (e.g., `~/Development/Personal/python-ml/`)
- **Auto-loaded agents**: `Sisyfreeus`, `python-ai-researcher`
- **Workflow**: `python-ai`
- **Context**: PyTorch patterns, NumPy optimization, algorithm implementation

### When you open a Haxe/Heaps project (e.g., `~/Projects/2d-game/`)
- **Auto-loaded agents**: `Sisyfreeus`, `gamedev-haxe`
- **Workflow**: `gamedev-haxe`
- **Context**: Heaps.io API, H3D graphics, asset pipelines, frame rate optimization

### When you open general tinkering
- **Auto-loaded agents**: `Sisyfreeus`, `explore-free`
- **Workflow**: (none - use ad-hoc)
- **Context**: General-purpose exploration

## Cost Strategy

All personal work uses **FREE models**:

| Model | Cost | Use Case |
|-------|------|----------|
| **Sisyfreeus (GLM-4.7)** | FREE | Primary coder for all work |
| **Planner-Sisyfreeus (GLM-4.7)** | FREE | Planning and architecture |
| **rust-ai-engineer (GLM-4.7)** | FREE | Rust-specific patterns |
| **python-ai-researcher (GLM-4.7)** | FREE | Python/ML algorithms |
| **debugger-free (Grok)** | FREE | Fast debugging |
| **librarian-free (Gemini Flash)** | ~$0.30/1M | Research and reference code |
| **oracle-free (Gemini Pro)** | ~$1.05/1M | Reserve for complex reasoning |

**Philosophy**: Default to FREE models (GLM-4.7, MiniMax M2.1, Grok). Only use Gemini Pro (`oracle-free`) for complex architectural decisions that really need the extra reasoning power.

## Key Agents

| Agent | Model | Best For |
|-------|-------|----------|
| **Sisyfreeus** | GLM-4.7 | Main work (all stacks) |
| **Planner-Sisyfreeus** | GLM-4.7 | Architecture and planning |
| **Builder-Sisyfreeus** | MiniMax M2.1 | Fast implementation |
| **rust-ai-engineer** | GLM-4.7 | Rust-specific work |
| **python-ai-researcher** | GLM-4.7 | Python/ML experiments |
| **gamedev-haxe** | Gemini Flash | Heaps.io game dev |
| **reasoner-free** | GLM-4.7 | Mathematical/algorithmic reasoning |
| **oracle-free** | Gemini Pro | Complex strategic decisions |
| **librarian-free** | Gemini Flash | Research external resources |
| **debugger-free** | Grok Code | Quick debugging (very fast) |
| **performance-optimizer-free** | GLM-4.7 | Performance analysis |

## Updating

To update agent definitions:

1. Edit `agents.json` in this directory
2. Changes are live immediately (OpenCode will reload)
3. Commit and push to sync across machines

To add a new workspace or language:

1. Add to `spaces.yaml` with path patterns and agents
2. Create corresponding workflow in `workflows.yaml`
3. Commit and push

## Related Documents

- See `/agent-reference/` for detailed agent personality docs
- See `/shared-concepts/` for cost optimization and flow routing
- See `RESTRUCTURE_PLAN.md` for overall architecture
- See `../claude/` for Kato work configuration (Claude Code)
