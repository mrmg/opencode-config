# AI Config Restructure Plan
## Claude Code + OpenCode Dual Setup

### Current State
```
opencode-config/
├── oh-my-opencode.json     (OpenCode agents + config)
├── opencode.jsonc          (OpenCode plugins/MCP)
├── agent/                  (Shared agent definitions)
├── command/                (OpenCode commands)
└── .gitignore
```

### Problem
- **Kato work uses Claude Code** (different config location: `~/.claude/`)
- **Personal work uses OpenCode** (current setup: `opencode-config/`)
- Currently no Claude-specific configuration
- No clear separation of shared vs. tool-specific agents

### Desired State
```
~/.claude/
├── agents.yaml             (Claude-specific agents)
├── workflows.yaml          (Claude workflows for Kato)
└── .clauderc              (Claude configuration)

~/.opencode/
├── agents/                 (OpenCode agents)
├── config.jsonc           (OpenCode plugins)
├── commands/              (OpenCode commands)
└── .opencodecrc           (OpenCode config)

opencode-config/ (DEPRECATED - migrate content above)
├── MIGRATION_GUIDE.md
└── archive/               (reference only)
```

---

## Phase 1: Create `.claude/` Directory Structure

Claude Code looks for agents in `~/.claude/agents.yaml` or `~/.claude/agents/` directory.

### New File Structure for Claude

Claude Code looks for config at `~/.claude/`.

Since you're using linking, the structure in the repo is:
```
opencode-config/claude/
├── agents.yaml                    # Claude agent definitions
├── workflows.yaml                # Claude workflows (Kato-specific)
├── workspace.yaml                # Kato workspace config
└── .clauderc                      # Claude runtime config

# Then link on each machine:
# ln -s /path/to/opencode-config/config/claude ~/.claude
```

#### `agents.yaml` Format
Claude uses a simpler format than OpenCode:

```yaml
agents:
  Sisyphus:
    role: "Cost-optimized main agent (Haiku 4.5)"
    capabilities:
      - code-analysis
      - implementation
      - multi-file-coordination
    temperature: 0.3
    context-size: 200k
    
  Planner-Sisyphus:
    role: "Planning with extended thinking"
    capabilities:
      - architecture
      - planning
      - reasoning
    temperature: 0.3
    
  Builder-Sisyphus:
    role: "Fast implementation"
    temperature: 0.2
    capabilities:
      - implementation
      - testing
      
  Architect-Opus:
    role: "Premium architecture (Kato-only)"
    capabilities:
      - complex-architecture
      - legacy-migrations
      - production-critical
    temperature: 0.2

  # Specialized agents for Kato
  backend-laravel:
    role: "Laravel backend specialist"
    specialization: "Laravel, Eloquent, middleware, testing"
    temperature: 0.2
    
  frontend-react-ts:
    role: "React/TypeScript specialist"
    specialization: "React, TypeScript, Tailwind, hooks"
    temperature: 0.3
    
  test-engineer:
    role: "Test generation and strategy"
    temperature: 0.2
    
  security-auditor:
    role: "Security code review"
    temperature: 0.1
    
  # ... other specialized agents
```

#### `workflows.yaml` Format
Kato-specific workflows (not relevant to personal work):

```yaml
workflows:
  monorepo-backend:
    agents:
      - Sisyphus              # Main agent
      - Architect-Opus        # For complex decisions
      - backend-laravel       # Specialized
      - test-engineer         # Testing
    conventions:
      orm: "eloquent"
      testing: "phpunit/pest"
      api: "REST + GraphQL"
    patterns:
      - database-migrations
      - middleware-design
      - service-container
      
  monorepo-frontend:
    agents:
      - Sisyphus
      - frontend-react-ts
      - frontend-ui-ux-engineer
    conventions:
      framework: "React 19"
      types: "TypeScript 5"
      styling: "Tailwind CSS"
      testing: "Jest + React Testing Library"
    patterns:
      - hook-dependencies
      - component-composition
      - state-management
      
  multi-test:
    agents:
      - Sisyphus
      - Architect-Opus
      - Coder-Sonnet45
    command: "/multi-test"
    description: "Parallel implementation comparison"
```

#### `workspace.yaml` Format
Describes the Kato codebase structure:

```yaml
workspace:
  name: "Kato - Monorepo Work Project"
  path: "/home/mrmg/Development/Kato"
  type: "monorepo"
  
  stacks:
    backend:
      language: "php"
      framework: "laravel"
      root: "backend/"
      conventions:
        - eloquent-orm
        - blade-templates
        - artisan-commands
        - middleware-based-auth
      
    frontend:
      language: "typescript"
      framework: "react"
      root: "frontend/"
      conventions:
        - react-hooks
        - typescript-strict
        - tailwind-css
        - nextjs-app-router
      
  integration:
    api-contract: "Laravel REST API → React TypeScript clients"
    shared: "types/ → shared TypeScript definitions"
    deployment: "Docker Compose (backend + frontend)"
```

---

## Phase 2: Migrate OpenCode Config to `~/.opencode/`

Move personal project configuration out of the generic config directory.

### New File Structure for OpenCode

OpenCode looks for config at `~/.config/opencode/` (or `~/.opencode/` depending on platform).

Since you're using linking, the structure in the repo is:
```
opencode-config/opencode/
├── agents.json                    # OpenCode agent definitions
├── config.jsonc                   # MCP/plugin config
├── workflows.yaml                # Personal workflows
├── spaces.yaml                    # Personal workspace routing
├── commands/
│   ├── worktrees/                # Multi-agent testing commands
│   │   ├── integrate-worktrees.md
│   │   ├── multi-test.md
│   │   └── review-worktrees.md
│   ├── personal/                 # Personal project commands
│   │   ├── new-component.md
│   │   ├── migrate-feature.md
│   │   └── test-perf.md
│   └── flows/                    # Language-specific workflows
│       ├── rust-ai.md
│       ├── python-ai.md
│       └── gamedev-haxe.md
└── .opencodecrc                   # OpenCode runtime config

# Then link on each machine:
# ln -s /path/to/opencode-config/opencode ~/.config/opencode
# OR
# ln -s /path/to/opencode-config/opencode ~/.opencode
```

### What Goes in `~/.opencode/agents.json`

```json
{
  "agents": {
    "__comment_personal": "Personal projects only (Rust, Python, Haxe)",
    
    "Sisyfreeus": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.3,
      "mode": "primary",
      "description": "Personal: PRIMARY CODER - 73.8% SWE (FREE)"
    },
    "Planner-Sisyfreeus": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.2,
      "mode": "primary",
      "description": "Personal: ARCHITECT - 73.8% SWE + reasoning"
    },
    "Builder-Sisyfreeus": {
      "model": "opencode/minimax-m2.1-free",
      "temperature": 0.2,
      "mode": "primary",
      "description": "Personal: BUILDER - 74% SWE, fastest"
    },
    
    "rust-ai-engineer": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.2,
      "mode": "subagent",
      "description": "Personal: Rust specialist (ownership, async, performance)"
    },
    "python-ai-researcher": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.2,
      "mode": "subagent",
      "description": "Personal: Python ML/AI (algorithms, PyTorch, JAX)"
    },
    "gamedev-haxe": {
      "model": "github-copilot/gpt-5-mini",
      "temperature": 0.5,
      "mode": "subagent",
      "description": "Personal: Heaps.io specialist (visual, performance)"
    },
    
    "oracle-free": {
      "model": "github-copilot/gpt-5-mini",
      "temperature": 0.2,
      "mode": "subagent",
      "description": "Personal: Strategic reasoning ($0.35/$1.05 per 1M)"
    },
    "reasoner-free": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.1,
      "mode": "subagent",
      "description": "Personal: Elite reasoning (95.7% AIME)"
    },
    
    "librarian-free": {
      "model": "github-copilot/gpt-5-mini",
      "temperature": 0.1,
      "mode": "subagent",
      "description": "Personal: Fast research"
    },
    "explore-free": {
      "model": "github-copilot/gpt-5-mini",
      "temperature": 0.0,
      "mode": "subagent",
      "description": "Personal: Codebase exploration (1M context)"
    },
    
    "test-engineer-free": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.2,
      "mode": "subagent",
      "description": "Personal: Test generation"
    },
    "debugger-free": {
      "model": "opencode/grok-code",
      "temperature": 0.2,
      "mode": "subagent",
      "description": "Personal: Fast debugging (296 tok/s)"
    },
    "performance-optimizer-free": {
      "model": "opencode/glm-4.7-free",
      "temperature": 0.2,
      "mode": "subagent",
      "description": "Personal: Performance optimization"
    },
    
    "frontend-free": {
      "model": "github-copilot/gpt-5-mini",
      "temperature": 0.7,
      "mode": "subagent",
      "description": "Personal: Creative UI (1M context)"
    },
    "document-writer-free": {
      "model": "github-copilot/gpt-5-mini",
      "temperature": 0.5,
      "mode": "subagent",
      "description": "Personal: Documentation"
    }
  },
  
  "sisyphus_agent": {
    "disabled": false,
    "default_builder_enabled": true,
    "planner_enabled": true,
    "replace_plan": true
  }
}
```

---

## Phase 3: Refactor `opencode-config/` as Single Source of Truth

Keep this repo as a **unified reference** but with clear markers of what applies where.

### New Structure in `opencode-config/`

```
opencode-config/
├── README.md                      # Overview of dual setup
├── RESTRUCTURE_PLAN.md           # This file
├── .gitignore
│
├── claude/                        # Symlinked to ~/.claude/
│   ├── agents.yaml               # Claude agent definitions
│   ├── workflows.yaml            # Claude workflows (Kato)
│   ├── workspace.yaml            # Kato workspace config
│   ├── .clauderc                 # Claude runtime config (optional)
│   └── README.md                 # Claude-specific notes
│
├── opencode/                      # Symlinked to ~/.config/opencode/ OR ~/.opencode/
│   ├── agents.json               # OpenCode agent definitions
│   ├── config.jsonc              # OpenCode plugins/MCP config
│   ├── workflows.yaml            # OpenCode workflows (personal)
│   ├── spaces.yaml               # OpenCode workspace routing
│   ├── commands/
│   │   ├── worktrees/
│   │   │   ├── integrate-worktrees.md
│   │   │   ├── multi-test.md
│   │   │   └── review-worktrees.md
│   │   ├── personal/
│   │   │   ├── new-component.md
│   │   │   ├── migrate-feature.md
│   │   │   └── test-perf.md
│   │   └── flows/
│   │       ├── rust-ai.md
│   │       ├── python-ai.md
│   │       └── gamedev-haxe.md
│   ├── .opencodecrc              # OpenCode runtime config (optional)
│   └── README.md                 # OpenCode-specific notes
│
├── agent-reference/               # Shared documentation (no symlinks)
│   ├── Sisyphus-haiku45.md
│   ├── Sisyfreeus.md
│   ├── Planner-Sisyfreeus.md
│   ├── Builder-Sisyfreeus.md
│   └── model-agents/
│
├── shared-concepts/
│   ├── workspace-detection.md
│   ├── flow-routing.md
│   └── cost-optimization.md
│
└── .local/                       # Machine-specific (not committed)
    └── work-laptop.yaml
```

---

## Phase 4: Smart Context Routing

Both Claude and OpenCode should detect workspace context automatically.

### For Claude (in `~/.claude/workspace.yaml`)

```yaml
workspace:
  detection:
    - pattern: "/home/mrmg/Development/Kato/**"
      workspace: "monorepo-backend"
      agents: [Sisyphus, Architect-Opus, backend-laravel]
      
    - pattern: "/home/mrmg/Development/Kato/frontend/**"
      workspace: "monorepo-frontend"
      agents: [Sisyphus, frontend-react-ts]
```

Claude will then:
1. Detect you're in Kato/backend/ → load Kato backend context
2. Suggest backend-laravel workflow
3. Know Eloquent patterns, middleware, etc.

### For OpenCode (in `~/.opencode/spaces.yaml`)

```yaml
spaces:
  - name: "Rust AI Projects"
    paths: ["/home/mrmg/Development/Personal/rust-**"]
    agents: [Sisyfreeus, rust-ai-engineer]
    default-flow: "rust-ai"
    
  - name: "Python AI Research"
    paths: ["/home/mrmg/Development/Personal/python-**"]
    agents: [Sisyfreeus, python-ai-researcher]
    default-flow: "python-ai"
    
  - name: "Haxe Game Dev"
    paths: ["/home/mrmg/Development/Personal/game-**"]
    agents: [Sisyfreeus, gamedev-haxe]
    default-flow: "gamedev-haxe"
```

OpenCode will then:
1. Detect you're in game-haxe/ → load gamedev context
2. Load gamedev-haxe agent
3. Auto-suggest frame-rate profiling commands

---

## Phase 5: File Mapping Reference

### What Moves Where

| Current | Claude | OpenCode | Reference |
|---------|--------|----------|-----------|
| `oh-my-opencode.json` | - | Becomes `~/.opencode/agents.json` | Keep copy in `opencode-config/opencode/agents.json` |
| `opencode.jsonc` | - | Becomes `~/.opencode/config.jsonc` | Keep copy in `opencode-config/opencode/config.jsonc` |
| `agent/Sisyphus-haiku45.md` | - | Keep in repo | Reference doc |
| `agent/Sisyfreeus.md` | - | Keep in repo | Reference doc |
| `agent/Planner-Sisyfreeus.md` | - | Keep in repo | Reference doc |
| `agent/Builder-Sisyfreeus.md` | - | Keep in repo | Reference doc |
| `agent/model-agents/` | - | Keep in repo | Reference docs |
| `command/worktrees/` | - | Becomes `~/.opencode/commands/worktrees/` | Keep copies |

### What's New

| File | Location | Purpose |
|------|----------|---------|
| `agents.yaml` | `~/.claude/` | Claude agent definitions |
| `workflows.yaml` | `~/.claude/` | Claude workflows (Kato) |
| `workspace.yaml` | `~/.claude/` | Kato workspace config |
| `agents.json` | `~/.opencode/` | OpenCode agent definitions |
| `workflows.yaml` | `~/.opencode/` | OpenCode workflows (personal) |
| `spaces.yaml` | `~/.opencode/` | OpenCode workspace routing |
| `new-component.md` | `~/.opencode/commands/personal/` | New component scaffolding |
| `migrate-feature.md` | `~/.opencode/commands/personal/` | Feature migration |
| `test-perf.md` | `~/.opencode/commands/personal/` | Performance testing |

---

## Phase 6: Setup Instructions (Using Symlinks)

### One-Time Setup Per Machine

Since you're using symlinks, setup is minimal:

#### Step 1: Clone/pull the config repo
```bash
cd /home/mrmg/Development/Build
git clone <your-config-repo> opencode-config
# OR if already exists:
cd opencode-config && git pull
```

#### Step 2: Create Claude symlink
```bash
# Claude looks for config at ~/.claude
ln -sf /home/mrmg/Development/Build/opencode-config/config/claude ~/.claude

# Verify:
ls -la ~/.claude/agents.yaml
```

#### Step 3: Create OpenCode symlink
```bash
# OpenCode looks for config at ~/.config/opencode
mkdir -p ~/.config
ln -sf /home/mrmg/Development/Build/opencode-config/opencode ~/.config/opencode

# OR if your system uses ~/.opencode:
ln -sf /home/mrmg/Development/Build/opencode-config/opencode ~/.opencode

# Verify:
ls -la ~/.config/opencode/agents.json
```

#### Step 4: Test with both tools
- Claude Code: Open Kato repo → should see agents from `~/.claude/agents.yaml`
- OpenCode: Open personal Rust project → should see agents from `~/.config/opencode/agents.json`

### Updating Configs (Painless!)

Since symlinks point to the repo, changes are immediate:

```bash
cd /home/mrmg/Development/Build/opencode-config

# Edit any config (live immediately in both tools)
vim claude/agents.yaml
vim opencode/agents.json

# Commit and push to sync across machines
git add .
git commit -m "Update Claude/OpenCode agents"
git push

# On other machines, just pull:
git pull
# Symlinks already point to repo, no re-linking needed!
```

### Optional: Machine-Specific Overrides

If you need machine-specific settings (e.g., different model on work laptop vs. desktop):

```bash
# Create .local/ directory (not committed to git)
mkdir -p opencode-config/.local

# Machine-specific configs (won't be committed)
cat > opencode-config/.local/work-laptop.yaml << 'EOF'
overrides:
  Sisyphus:
    model: "claude-sonnet-4-5"
  backend-laravel:
    model: "claude-opus-4.5"
EOF

# Add to .gitignore
echo ".local/" >> opencode-config/.gitignore
```

Then reference in main config (source mechanism TBD based on Claude/OpenCode support).

---

## Benefits of This Structure

### ✅ For Claude Code (Kato Work)
- **Focused agents**: Only see Kato-relevant agents (no personal Haxe stuff)
- **Context awareness**: Knows Laravel patterns, React conventions, test frameworks
- **Workflow optimization**: Kato-specific workflows (monorepo-backend, monorepo-frontend)
- **Cost-managed**: Haiku 4.5 baseline, Opus for migrations only

### ✅ For OpenCode (Personal Work)
- **Language-specific**: rust-ai-engineer for Rust, python-ai-researcher for Python, gamedev-haxe for Heaps
- **Cost-free**: Default to GLM-4.7 (FREE), reserve credits for complex reasoning
- **Space-aware**: Automatically load right agents based on project directory
- **Commands**: New commands for scaffolding, migrations, perf testing

### ✅ For Both Tools
- **Unified reference**: opencode-config/ is single source of truth
- **Easy updates**: Change one file, both tools see it
- **Clear separation**: No confusion about what applies to Kato vs. personal
- **Git-friendly**: Both configs in version control, synced to repo

---

## Next Steps

1. Create `/opencode-config/claude/` directory with YAML configs
2. Create `/opencode-config/opencode/` directory with JSON configs
3. Create migration guide in `/opencode-config/MIGRATION_GUIDE.md`
4. Document in `/opencode-config/README.md`
5. Update `.gitignore` if needed (both `~/.claude` and `~/.opencode` can be local)
6. Test with both Claude and OpenCode

