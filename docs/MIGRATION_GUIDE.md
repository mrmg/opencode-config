# Claude + OpenCode Configuration Migration Guide

This guide walks you through setting up your dual-tool AI config using symlinks.

## Overview

You use:
- **Claude Code** for Kato (work) - production monorepo with Laravel/React
- **OpenCode** for Personal projects - Rust AI, Python ML, Haxe game dev

This repo contains configuration for both:
- `claude/` → symlink to `~/.claude/` (Claude Code config)
- `opencode/` → symlink to `~/.config/opencode/` (OpenCode config)

## Prerequisites

- Repo cloned to: `/home/mrmg/Development/Build/opencode-config`
- Claude Code installed (work machine)
- OpenCode installed (personal machine or same machine)
- Bash shell (for symlink commands)

## Step 1: Verify Directory Structure

```bash
cd /home/mrmg/Development/Build/opencode-config

# Check that directories exist:
ls -la claude/        # Should have agents.yaml, workflows.yaml, workspace.yaml
ls -la opencode/      # Should have agents.json, config.jsonc, workflows.yaml, spaces.yaml, commands/
```

Expected output:
```
claude/:
  -rw-r--r--  agents.yaml
  -rw-r--r--  workflows.yaml
  -rw-r--r--  workspace.yaml
  -rw-r--r--  README.md

opencode/:
  -rw-r--r--  agents.json
  -rw-r--r--  config.jsonc
  -rw-r--r--  workflows.yaml
  -rw-r--r--  spaces.yaml
  -rw-r--r--  README.md
  drwxr-xr-x  commands/
```

## Step 2: Set Up Claude Configuration (Work Machine)

### Create Claude symlink

```bash
# Create symlink pointing to claude/ directory
ln -sf /home/mrmg/Development/Build/opencode-config/claude ~/.claude

# Verify symlink was created
ls -la ~/.claude

# Verify it contains the config
cat ~/.claude/agents.yaml | head -20
```

Expected output: `~/.claude -> /home/mrmg/Development/Build/opencode-config/claude`

### Test Claude Code detects the config

1. Open Claude Code
2. Open your Kato repository
3. Look for agent suggestions - should see:
   - `Sisyphus` (primary)
   - `Planner-Sisyphus`
   - `Builder-Sisyphus`
   - `backend-laravel` (for backend files)
   - `frontend-react-ts` (for frontend files)

If agents don't appear:
- Verify `~/.claude/agents.yaml` exists: `cat ~/.claude/agents.yaml`
- Restart Claude Code
- Check Claude settings for agent discovery path

## Step 3: Set Up OpenCode Configuration (Personal Machine)

### Determine your OpenCode config location

OpenCode uses different paths on different systems:

```bash
# Check where OpenCode looks for config:
echo $HOME/.config/opencode     # Most Linux/Mac systems
echo $HOME/.opencode            # Alternative location

# If unsure, create both (one will be redundant but harmless):
mkdir -p ~/.config/opencode
mkdir -p ~/.opencode
```

### Create OpenCode symlinks

```bash
# Most common: ~/.config/opencode
ln -sf /home/mrmg/Development/Build/opencode-config/opencode ~/.config/opencode

# Verify
ls -la ~/.config/opencode/agents.json

# OR if your system uses ~/.opencode:
ln -sf /home/mrmg/Development/Build/opencode-config/opencode ~/.opencode
ls -la ~/.opencode/agents.json
```

### Test OpenCode detects the config

1. Open OpenCode
2. Open one of your personal projects (e.g., Rust AI project)
3. Look for agent suggestions - should see:
   - `Sisyfreeus` (primary)
   - Language specialist (e.g., `rust-ai-engineer` for Rust, `python-ai-researcher` for Python)

If agents don't appear:
- Verify config location: `ls ~/.config/opencode/agents.json` or `ls ~/.opencode/agents.json`
- Restart OpenCode
- Check OpenCode settings for agent discovery

## Step 4: Verify Both Tools Work

### Test Claude Code

**In Kato repository:**

```bash
# Open a backend file
cd /home/mrmg/Development/Kato
code backend/app/Services/UserService.php
```

Expected behavior:
- Claude Code loads
- See agent panel with `Sisyphus`, `backend-laravel`, `test-engineer` available
- Open workflow should show "monorepo-backend"

```bash
# Open a frontend file
code frontend/src/components/Auth.tsx
```

Expected behavior:
- Claude Code loads
- See agent panel with `Sisyphus`, `frontend-react-ts`, `frontend-ui-ux-engineer` available
- Workflow should show "monorepo-frontend"

### Test OpenCode

**In a personal project:**

```bash
# Open a Rust project
cd /home/mrmg/Development/Personal/rust-algorithms
code .
```

Expected behavior:
- OpenCode loads
- See `Sisyfreeus` and `rust-ai-engineer` agents available
- Workflow should show "rust-ai"

```bash
# Open a Python project
cd /home/mrmg/Development/Personal/python-ml
code .
```

Expected behavior:
- OpenCode loads
- See `Sisyfreeus` and `python-ai-researcher` agents available
- Workflow should show "python-ai"

## Step 5: Keep Configs in Sync Across Machines

Since symlinks point to the repo, syncing is automatic:

### On any machine, update a config:

```bash
cd /home/mrmg/Development/Build/opencode-config

# Make changes
vim claude/agents.yaml
vim opencode/agents.json

# Commit and push
git add .
git commit -m "Update agents: add new rust-api-layer specialist"
git push
```

### On other machines, pull the changes:

```bash
cd /home/mrmg/Development/Build/opencode-config
git pull

# Changes are live immediately (symlinks already point to repo!)
# No need to re-create symlinks or restart tools
```

Tools will pick up changes after reload/restart, but no manual setup needed.

## Step 6: (Optional) Machine-Specific Overrides

If you want machine-specific settings without committing them:

```bash
# Create .local directory (not committed)
mkdir -p /home/mrmg/Development/Build/opencode-config/.local

# Add to .gitignore
echo ".local/" >> /home/mrmg/Development/Build/opencode-config/.gitignore

# Create machine-specific config
cat > /home/mrmg/Development/Build/opencode-config/.local/work-laptop.yaml << 'EOF'
overrides:
  Sisyphus:
    model: "claude-sonnet-4-5"  # Faster model on work machine
  backend-laravel:
    model: "claude-opus-4.5"    # Worth the cost for critical backend work
EOF
```

Then reference in main config (mechanism depends on Claude/OpenCode support for includes).

## Step 7: Update .gitignore (Optional)

If you add `.local/` for machine-specific overrides:

```bash
cd /home/mrmg/Development/Build/opencode-config
echo ".local/" >> .gitignore
git add .gitignore
git commit -m "Add .local/ to gitignore for machine-specific overrides"
git push
```

## Troubleshooting

### Claude Code doesn't see agents

**Problem**: Claude Code opens but agents don't appear in the sidebar

**Solution**:
1. Verify symlink exists: `ls -la ~/.claude`
2. Verify agents file: `cat ~/.claude/agents.yaml | head -5`
3. Check Claude Code settings for agent discovery path
4. Restart Claude Code completely (kill process, reopen)
5. Check if Claude expects different file format (YAML vs JSON vs other)

**Last resort**: Copy instead of symlink
```bash
cp -r /home/mrmg/Development/Build/opencode-config/claude ~/.claude
# But now changes won't sync automatically!
```

### OpenCode doesn't auto-load workspace agents

**Problem**: OpenCode opens but doesn't suggest context-specific agents

**Solution**:
1. Verify config location: `ls ~/.config/opencode/spaces.yaml` or `ls ~/.opencode/spaces.yaml`
2. Check path patterns in `spaces.yaml` match your actual project paths
3. Verify agents are defined in `agents.json`
4. Restart OpenCode
5. Try manually selecting agent from dropdown to confirm agents are available

### Symlinks not working on Windows

**Problem**: Windows doesn't support symlinks by default

**Solution**: Use directory junctions instead
```powershell
# PowerShell (admin)
cmd /c mklink /J "$env:USERPROFILE\.claude" "C:\path\to\opencode-config\claude"
cmd /c mklink /J "$env:USERPROFILE\.config\opencode" "C:\path\to\opencode-config\opencode"
```

Or just copy directories:
```powershell
Copy-Item -Recurse "opencode-config\claude" "$env:USERPROFILE\.claude"
Copy-Item -Recurse "opencode-config\opencode" "$env:USERPROFILE\.config\opencode"
```

**Caveat**: Changes won't sync automatically; you'll need to copy regularly.

### Changes in repo not showing up in Claude/OpenCode

**Problem**: You edited a config file but the tool doesn't reflect changes

**Solution**:
1. Verify file was saved: `cat ~/.claude/agents.yaml` or `cat ~/.config/opencode/agents.json`
2. Restart the tool (full restart, not just reloading a project)
3. If using copies (not symlinks), you need to manually copy updated files

## File Organization Reference

### Claude Configuration

| File | Purpose |
|------|---------|
| `claude/agents.yaml` | Agent definitions (Sisyphus, Architect-Opus, specialists) |
| `claude/workflows.yaml` | Workflows (monorepo-backend, monorepo-frontend, api-contract) |
| `claude/workspace.yaml` | Kato workspace structure, context routing, cost strategy |
| `claude/README.md` | Documentation for Claude setup |

### OpenCode Configuration

| File | Purpose |
|------|---------|
| `opencode/agents.json` | Agent definitions (Sisyfreeus, language specialists) |
| `opencode/config.jsonc` | Plugins and MCP servers |
| `opencode/workflows.yaml` | Workflows (rust-ai, python-ai, gamedev-haxe) |
| `opencode/spaces.yaml` | Workspace routing and auto-detection |
| `opencode/README.md` | Documentation for OpenCode setup |
| `opencode/commands/` | Custom commands (worktrees, personal, flows) |

### Shared Documentation

| File | Purpose |
|------|---------|
| `agent-reference/` | Agent personality docs (Sisyphus, Sisyfreeus, model-agents) |
| `shared-concepts/` | Cost optimization, workspace detection, flow routing |
| `RESTRUCTURE_PLAN.md` | High-level architecture and design rationale |
| `MIGRATION_GUIDE.md` | This file! |

## Next Steps

After migration:

1. **Try your first task with Claude Code**: Open Kato, edit a backend file, use Sisyphus agent
2. **Try your first task with OpenCode**: Open a Rust project, use rust-ai-engineer agent
3. **Test workspace detection**: Verify agents auto-load based on file paths
4. **Commit a change**: Edit a config, push, pull on another machine, verify sync

## Support

If you encounter issues:

1. Check the Troubleshooting section above
2. Review the README files in `claude/` and `opencode/` directories
3. Verify symlinks are pointing to correct locations
4. Check that tool paths in agents match your actual model names
5. Ensure both tools are running latest versions

