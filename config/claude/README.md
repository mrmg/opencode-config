# Claude Configuration for Kato Work

This folder is symlinked to `~/.claude/` on your work machine.

## Files in This Directory

- **agents.yaml** - Agent definitions for Claude Code
  - `Sisyphus` (Haiku 4.5) - Primary agent
  - `Planner-Sisyphus`, `Builder-Sisyphus` - Role variants
  - `Architect-Opus` - Premium agent for critical work
  - Stack specialists: `backend-laravel`, `frontend-react-ts`
  - Utility agents: test-engineer, security-auditor, database-architect, etc.

- **workflows.yaml** - Workflow definitions for Kato
  - `monorepo-backend` - Laravel development workflow
  - `monorepo-frontend` - React/TypeScript development workflow
  - `api-contract` - Backend ↔ Frontend integration
  - `security-audit` - Security review workflow
  - `multi-test` - Parallel implementation testing

- **workspace.yaml** - Kato workspace configuration
  - Backend stack details (Laravel, Eloquent, PHPUnit)
  - Frontend stack details (React, TypeScript, Next.js, Tailwind)
  - Integration points (API contract, shared types)
  - Context routing (auto-load right agents based on file path)
  - Cost optimization strategy
  - Performance targets

## Setup

Create symlink from this directory to Claude's config location:

```bash
ln -sf /path/to/opencode-config/claude ~/.claude
```

## Usage

When you open a file in Claude Code:

### Backend File (e.g., `backend/app/Services/UserService.php`)
- **Auto-loaded agents**: `Sisyphus`, `backend-laravel`, `test-engineer`
- **Workflow**: `monorepo-backend`
- **Context**: Eloquent patterns, Laravel middleware, PHPUnit testing

### Frontend File (e.g., `frontend/src/components/Auth.tsx`)
- **Auto-loaded agents**: `Sisyphus`, `frontend-react-ts`, `frontend-ui-ux-engineer`
- **Workflow**: `monorepo-frontend`
- **Context**: React hooks, TypeScript strict mode, Tailwind CSS

### API Contract File (e.g., `types/api.ts`)
- **Auto-loaded agents**: `Sisyphus`, `api-designer`, `backend-laravel`, `frontend-react-ts`
- **Workflow**: `api-contract`
- **Context**: Type safety, schema validation, REST standards

## Updating

To update agent definitions:

1. Edit `agents.yaml` in this directory
2. Changes are live immediately (Claude will reload)
3. Commit and push to sync across machines

To add a new workflow:

1. Edit `workflows.yaml` with new workflow definition
2. Add corresponding context routing in `workspace.yaml`
3. Commit and push

## Cost Management

- **Default**: Haiku 4.5 ($1/$5 per 1M tokens) - 73.3% SWE-bench
- **Migrations**: Escalate to Architect-Opus ($5/$25 per 1M tokens) - 80.9% SWE-bench
- **Philosophy**: Fast iteration at Haiku baseline, premium cost justified only for production-critical decisions

## Key Agents

| Agent | Model | Best For |
|-------|-------|----------|
| **Sisyphus** | Haiku 4.5 | Main work (code, analysis, implementation) |
| **Planner-Sisyphus** | Haiku 4.5 | Planning complex features |
| **Builder-Sisyphus** | Haiku 4.5 | Fast implementation |
| **Architect-Opus** | Opus 4.5 | Complex migrations, critical architecture |
| **backend-laravel** | Haiku 4.5 | Laravel-specific patterns |
| **frontend-react-ts** | Haiku 4.5 | React/TypeScript edge cases |
| **test-engineer** | Gemini Flash | Test generation |
| **security-auditor** | Gemini Flash | Security review |
| **performance-optimizer** | Gemini Pro | Performance analysis |

## Related Documents

- See `/agent-reference/` for detailed agent personality docs
- See `/shared-concepts/` for cost optimization and flow routing
- See `RESTRUCTURE_PLAN.md` for overall architecture
