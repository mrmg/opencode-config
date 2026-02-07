# Agent Configuration Guide

This document describes how to use the opencode agent system with the orchestrator pattern.

## 3-Tier Strategy

| Tier | Provider | When to Use | Cost |
|------|----------|-------------|------|
| **Daily Driver** | **Chutes.ai** | Day-to-day coding, chat, subagents — best value open models | Subscription (pennies/task) |
| **Frontier** | **GitHub Copilot** | When you need the best (Opus, GPT, Gemini) — bundled flat rate crushes pay-per-token | $10-39/month flat |
| **Experimentation** | **OpenRouter** | Testing specific models, comparing outputs, using credits | Pay-per-token (spend credits) |

**Why this order:** A single heavy Opus 4.6 session on OpenRouter costs $3-10+.
Copilot Pro bundles dozens of premium requests/month for $10-39 flat. For frontier
models, Copilot is 5-10x better value. Chutes open models (DeepSeek V3.2, Kimi K2.5)
give 90% of frontier quality at 1/50th the cost for daily work.

## Quick Start

**Default Agent:** `assistant-fast` (Kimi K2.5 TEE via Chutes.ai)

**For daily coding:** Use `personal-coder` (Kimi K2.5 on Chutes) or `assistant-fast`
**For frontier quality:** Use GitHub Copilot (flat rate, best economics)
**For model testing:** Use `kato-*` agents or OpenRouter subagents (spend credits)

**To switch agents:**
```
/use personal-coder       # Daily driver (Chutes, best value)
/use assistant-fast       # General chat (Chutes)
/use kato-coder           # Copilot frontier (flat rate)
```

**To use a subagent via orchestrator:**
```
Use deepseek-v3-2 subagent to analyze this (budget, Chutes)
Use pony-alpha subagent for free frontier-class work (FREE, OpenRouter)
Use gemini-3-flash subagent for fast reasoning (value, OpenRouter)
Use claude-opus-4-6 subagent for deep analysis (frontier, OpenRouter — or just use Copilot)
```

## Pricing Reference (per 1M tokens: input/output)

| Tier | Model | Price | Context | Provider |
|------|-------|-------|---------|----------|
| **Frontier** | Claude Opus 4.6 | $5/$25 | 1M | OpenRouter |
| **Frontier** | GPT-5.2 Codex | $1.75/$14 | 400K | OpenRouter |
| **Frontier** | Gemini 3 Pro | $2/$12 | 1M | OpenRouter |
| **Strong** | Claude Sonnet 4.5 | $3/$15 | 1M | OpenRouter |
| **Value** | Kimi K2.5 | $0.45/$2.25 | 262K | Chutes |
| **Value** | Gemini 3 Flash | $0.50/$3 | 1M | OpenRouter |
| **Budget** | DeepSeek V3.2 | $0.25/$0.38 | 163K | Chutes |
| **Budget** | GPT-5 Mini | $0.25/$2 | 400K | OpenRouter |
| **Budget** | Devstral 2 | ~$0.05/$0.22 | 256K | Chutes |
| **Free** | Pony Alpha | FREE | 200K | OpenRouter (data logged!) |
| **Free** | DeepSeek R1 0528 | FREE | 163K | OpenRouter |
| **Free** | MiMo V2 Flash | FREE-tier | 256K | Chutes |

## Primary Agents

### Work (Kato) - Frontier Models via GitHub Copilot

**NOTE:** Kato agents use GitHub Copilot for frontier models at flat rate pricing.
Copilot Pro ($10-39/month) bundles premium requests — dramatically better value than
OpenRouter's pay-per-token (one heavy Opus session on OpenRouter costs $3-10+).

**IMPORTANT:** Do NOT use Pony Alpha for Kato — data is logged.

| Agent | Model | Provider | Price | Best For |
|-------|-------|----------|-------|----------|
| `kato-architect` | Claude Opus 4.6 | Copilot | Flat rate | System design, 1M context, architecture |
| `kato-coder` | GPT-5.2 Codex | Copilot | Flat rate | Daily coding, 400K context |
| `kato-strategist` | Claude Opus 4.6 | Copilot | Flat rate | Complex analysis, reasoning |
| `kato-typescript` | GPT-5.2 Codex | Copilot | Flat rate | TypeScript/React projects |

### Chat/Assistant Agents - Chutes.ai

| Agent | Model | Fallback | Best For |
|-------|-------|----------|----------|
| `assistant` | Kimi K2.5 TEE | GLM-4.7 Flash | General AI chat, 262K context |
| `assistant-fast` | **Kimi K2.5 TEE** | GLM-4.7 Flash | **PRIMARY DEFAULT - Best for all tasks** |

### Personal - Chutes.ai (Open Models)

| Agent | Model | Best For |
|-------|-------|----------|
| `personal-architect` | DeepSeek V3.2 TEE | Architecture (GPT-5 class, incredible value) |
| `personal-coder` | Kimi K2.5 TEE | Side projects, personal coding |
| `personal-strategist` | DeepSeek R1 0528 TEE | Research, analysis, reasoning |

### Special Purpose

| Agent | Model | Provider | Best For |
|-------|-------|----------|----------|
| `orchestrator` | Kimi K2.5 TEE | Chutes | Delegating tasks to specific models |
| `free-primary` | GLM-4.7 Flash | Chutes | When primary models unavailable |
| `free-rapid` | GLM-4.7 Flash | Chutes | Quick tasks, high speed |

## Subagent Mappings - Chutes + OpenRouter for Orchestration

Use these with the orchestrator: "Use `<subagent>` subagent to <task>"

### Model Comparison Orchestration

Run multiple subagents on the same task and compare results:

```
/use orchestrator
Run architect-claude, architect-codex, and architect-pony subagents 
to design a caching layer. Compare their approaches and synthesize the best solution.
```

### Architecture & Design Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `architect-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | Deep reasoning, 1M context |
| `architect-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Implementation-focused design |
| `architect-gemini` | **OpenRouter/Gemini 3 Pro** | $2/$12 | 1M context, multimodal |
| `architect-pony` | **OpenRouter/Pony Alpha** | FREE | Opus-class, data logged |
| `architect-deepseek` | **Chutes/DeepSeek V3.2** | $0.25/$0.38 | GPT-5 class, incredible value |
| `architect-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript/React focused |
| `architect-qwen` | **Chutes/Qwen3 235B** | Chutes sub | Large context, general |

### Implementation/Coding Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `coder-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Frontier coding, 400K context |
| `coder-codex-creative` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Higher temp for creative solutions |
| `coder-sonnet` | **OpenRouter/Sonnet 4.5** | $3/$15 | Strong coding/agents |
| `coder-pony` | **OpenRouter/Pony Alpha** | FREE | Opus-class coding, data logged |
| `coder-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | Best for TypeScript/React |
| `coder-deepseek` | **Chutes/DeepSeek V3.2** | $0.25/$0.38 | GPT-5 class at budget price |
| `coder-devstral` | **Chutes/Devstral 2** | ~$0.05/$0.22 | 123B agentic coding |
| `coder-qwen` | **Chutes/Qwen3 Coder** | Chutes sub | Agentic coding specialist |
| `coder-glm` | **Chutes/GLM-4.7 Flash** | Chutes sub | Fast, cost-effective |

### Code Review Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `reviewer-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | Deep analysis, quality |
| `reviewer-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Code-focused review |
| `reviewer-pony` | **OpenRouter/Pony Alpha** | FREE | Opus-class review, data logged |
| `reviewer-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript/React review |
| `reviewer-deepseek` | **Chutes/DeepSeek V3.2** | $0.25/$0.38 | Budget review, GPT-5 class |
| `reviewer-qwen` | **Chutes/Qwen3 Coder** | Chutes sub | Agentic review |

### Security Review Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `security-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | Threat modeling, deep analysis |
| `security-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Code security patterns |
| `security-deepseek` | **Chutes/DeepSeek R1** | Chutes sub | Vulnerability assessment |

### Documentation Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `docs-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | High-quality technical docs |
| `docs-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Code documentation, API docs |
| `docs-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript/React docs |
| `docs-qwen` | **Chutes/Qwen3 235B** | Chutes sub | Large context, comprehensive |
| `docs-glm` | **Chutes/GLM-4.7 Flash** | Chutes sub | Fast, cost-effective |

### Testing Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `tester-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Unit/integration tests |
| `tester-claude` | **OpenRouter/Sonnet 4.5** | $3/$15 | Comprehensive test strategy |
| `tester-qwen` | **Chutes/Qwen3 Coder** | Chutes sub | Agentic test generation |
| `tester-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript test patterns |

### Debugging Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `debugger-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Fast debugging |
| `debugger-deepseek` | **Chutes/DeepSeek V3.2** | $0.25/$0.38 | Deep root cause analysis |
| `debugger-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript/React debugging |
| `debugger-glm` | **Chutes/GLM-4.7 Flash** | Chutes sub | Quick error analysis |

### Optimization Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `optimizer-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | System-level optimization |
| `optimizer-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Performance profiling |
| `optimizer-deepseek` | **Chutes/DeepSeek V3.2** | $0.25/$0.38 | Algorithmic optimization |
| `optimizer-qwen` | **Chutes/Qwen3 Coder** | Chutes sub | Agentic performance tuning |

### Research & Analysis Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `researcher-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | Deep analysis, 1M context |
| `researcher-gemini` | **OpenRouter/Gemini 3 Pro** | $2/$12 | 1M context, multimodal |
| `researcher-deepseek` | **Chutes/DeepSeek R1** | Chutes sub | Reasoning-based research |
| `researcher-qwen` | **Chutes/Qwen3 235B** | Chutes sub | Large context, broad coverage |
| `researcher-glm` | **Chutes/GLM-4.7 Flash** | Chutes sub | Fast research |

### Refactoring Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `refactorer-claude` | **OpenRouter/Claude Opus 4.6** | $5/$25 | Complex refactoring |
| `refactorer-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Mechanical cleanup |
| `refactorer-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript/React refactoring |

### QA & Edge Cases Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `qa-claude` | **OpenRouter/Sonnet 4.5** | $3/$15 | Comprehensive test planning |
| `qa-qwen` | **Chutes/Qwen3 Coder** | Chutes sub | Agentic QA planning |
| `qa-glm` | **Chutes/GLM-4.7 Flash** | Chutes sub | Fast QA planning |

### Frontend/UI Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `frontend-claude` | **OpenRouter/Sonnet 4.5** | $3/$15 | UI/UX design, visual polish |
| `frontend-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | TypeScript/React UI |
| `frontend-qwen` | **Chutes/Qwen3 Coder** | Chutes sub | Component architecture |

### TypeScript Specialist Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `typescript-codex` | **OpenRouter/GPT-5.2 Codex** | $1.75/$14 | Frontier model for TS/React |
| `typescript-kimi` | **Chutes/Kimi K2.5** | $0.45/$2.25 | Best Chutes model for TS/React |

### Release Notes & Changelog Subagents

| Subagent | Provider/Model | Price | Description |
|----------|---------------|-------|-------------|
| `release-claude` | **OpenRouter/Sonnet 4.5** | $3/$15 | Changelog generation |
| `release-qwen` | **Chutes/Qwen3 235B** | Chutes sub | Version documentation |
| `release-glm` | **Chutes/GLM-4.7 Flash** | Chutes sub | Fast changelog |

### Direct Model Subagents

Access specific models directly via orchestrator:

| Subagent | Model | Provider | Price | Description |
|----------|-------|----------|-------|-------------|
| `pony-alpha` | Pony Alpha | OpenRouter | **FREE** | Stealth frontier, Opus-class. **Data logged — personal only** |
| `claude-opus-4-6` | Claude Opus 4.6 | OpenRouter | $5/$25 | Frontier reasoning, 1M context |
| `claude-sonnet-4-5` | Claude Sonnet 4.5 | OpenRouter | $3/$15 | Strong coding/agents, 1M context |
| `gpt-5-2-codex` | GPT-5.2 Codex | OpenRouter | $1.75/$14 | Frontier coding, 400K context |
| `gpt-5-mini` | GPT-5 Mini | OpenRouter | $0.25/$2 | Compact reasoning, 400K context |
| `gemini-3-pro` | Gemini 3 Pro | OpenRouter | $2/$12 | Frontier multimodal, 1M context |
| `gemini-3-flash` | Gemini 3 Flash | OpenRouter | $0.50/$3 | Near-Pro reasoning, 1M context |
| `deepseek-r1-free` | DeepSeek R1 0528 | OpenRouter | FREE | Strong reasoning, zero cost |
| `deepseek-v3-2` | DeepSeek V3.2 TEE | Chutes | $0.25/$0.38 | GPT-5 class, THE value king |
| `deepseek-r1-0528` | DeepSeek R1 0528 TEE | Chutes | Chutes sub | Best open reasoning |
| `kimi-k2-5` | Kimi K2.5 TEE | Chutes | $0.45/$2.25 | Agent swarm, 262K context |
| `kimi-k2-thinking` | Kimi K2 Thinking TEE | Chutes | Chutes sub | Reasoning mode |
| `qwen3-235b` | Qwen3 235B | Chutes | Chutes sub | Large context, general |
| `qwen3-coder-next` | Qwen3 Coder Next | Chutes | Chutes sub | Agentic coding |
| `glm-4-7` | GLM-4.7 Flash | Chutes | Chutes sub | Fast, cost-effective |
| `glm-4-7-tee` | GLM-4.7 TEE | Chutes | Chutes sub | Multimodal |
| `minimax-m2-1` | MiniMax M2.1 TEE | Chutes | Chutes sub | Lightweight coding |
| `devstral-2` | Devstral 2 123B | Chutes | ~$0.05/$0.22 | Agentic coding |
| `mimo-v2-flash` | MiMo V2 Flash | Chutes | FREE-tier | #1 open-source SWE-bench |

### Utility Subagents

| Subagent | Model | Provider | Description |
|----------|-------|----------|-------------|
| `rapid` | GLM-4.7 Flash | Chutes | Quick tasks, fast responses |
| `multimodal` | GLM-4.7 TEE | Chutes | Image analysis, vision tasks |
| `reasoner` | DeepSeek R1 0528 TEE | Chutes | Complex problem solving |
| `chat` | Claude Opus 4.6 | OpenRouter | General chat ($5/$25) |
| `chat-fast` | Kimi K2.5 TEE | Chutes | Fast chat |
| `chat-cheap` | GLM-4.7 Flash | Chutes | Cheap chat |

## Usage Examples

### Example 1: Work Task - Architecture Decision
```
/use kato-architect
I need to redesign our authentication system to support OAuth2, SAML, and SSO.
```

### Example 2: Free Frontier-Class Work (Personal)
```
/use orchestrator
Use pony-alpha subagent to design a caching layer. (FREE, Opus-class, data logged)
```

### Example 3: Budget Architecture (90% quality, 1/100th cost)
```
/use personal-architect
Design a caching layer for our API. (Uses DeepSeek V3.2 on Chutes)
```

### Example 4: Model Comparison
```
/use orchestrator
Run architect-claude ($5/$25), architect-pony (FREE), and architect-deepseek ($0.25/$0.38) 
to design a rate limiting system. Compare approaches.
```

### Example 5: Cost-Effective Coding
```
/use orchestrator
Use coder-deepseek subagent to implement this feature. (GPT-5 class at $0.25/$0.38)
```

### Example 6: Free Reasoning
```
/use orchestrator
Use deepseek-r1-free subagent to analyze this complex algorithm. (FREE via OpenRouter)
```

### Example 7: Security Review
```
/use orchestrator
Use security-deepseek subagent to review the auth module. (Chutes, best value)
```

### Example 8: Quick Task
```
/use free-rapid
Quickly summarize the changes in the last 5 commits.
```

### Example 9: General Chat (DEFAULT)
```
/use assistant-fast
Explain how React Server Components work.
```

## Temperature Settings

All agents use optimized temperatures:
- **0.0-0.2**: Coding, reasoning, analysis (deterministic)
- **0.3-0.5**: Documentation, creative tasks
- **0.7+**: UI/UX design, chat, highly creative work

## Configuration Files

- **Main config**: `~/.config/opencode/opencode.jsonc`
- **This guide**: `~/.config/opencode/AGENTS.md`

## Provider Configuration

### Supported Providers

1. **Chutes.ai** — Daily driver for open/open-source models
   - Serverless compute, TEE support
   - Best value via subscription
   - Use for: DeepSeek, Kimi, Qwen, GLM, MiniMax, Devstral, MiMo

2. **GitHub Copilot** — Frontier models at flat rate
   - Pro ($10/mo) or Pro+ ($39/mo)
   - Premium requests on Claude Opus, GPT, Gemini, etc.
   - Use for: Any frontier task where per-token would cost $1-10+
   - One heavy Opus session on OpenRouter ≈ entire month of Copilot Pro

3. **OpenRouter** — Experimentation & model testing
   - Unified API to Anthropic, OpenAI, Google, xAI
   - Pay-per-token pricing (some models free)
   - Use for: Spending credits, testing specific models, comparing outputs

### Authentication

```bash
# Chutes.ai (Daily driver - open models)
export CHUTES_API_KEY="your-chutes-api-key"

# OpenRouter (Experimentation - spend credits)
export OPENROUTER_API_KEY="your-openrouter-api-key"
```

## Model Selection Flowchart

```
What kind of task?
│
├── Daily coding / general work
│   └── Use Chutes (best value)
│       ├── Kimi K2.5 → Excellent coding ($0.45/$2.25)
│       ├── DeepSeek V3.2 → GPT-5 class ($0.25/$0.38) ← VALUE KING
│       └── Devstral 2 → Near-free agentic coding
│
├── Need frontier quality (complex architecture, hard bugs)
│   └── Use GitHub Copilot (flat rate, best economics)
│       └── One Opus session on OpenRouter costs $3-10+
│           That's an entire MONTH of Copilot Pro ($10)
│
├── Testing / comparing models (spending credits)
│   └── Use OpenRouter (kato-* agents or subagents)
│       ├── Claude Opus 4.6 ($5/$25) — best reasoning
│       ├── GPT-5.2 Codex ($1.75/$14) — best agentic coding
│       └── Gemini 3 Pro ($2/$12) — best multimodal
│
└── Free experimentation (personal, non-sensitive)
    ├── Pony Alpha (OpenRouter) — Opus-class, FREE (data logged!)
    ├── DeepSeek R1 free (OpenRouter) — Strong reasoning
    └── MiMo V2 Flash (Chutes) — #1 open-source SWE-bench
```

## Top Models 2026

### Best Value (Cost-to-Performance)
1. **Pony Alpha** (OpenRouter) — FREE, Opus 4.6 class, 200K context. Data logged — personal only.
2. **DeepSeek V3.2** (Chutes) — $0.25/$0.38, GPT-5 class, 163K context. 100x cheaper than Claude Opus.
3. **Kimi K2.5** (Chutes) — $0.45/$2.25, agent swarm, 262K context. Open-source.
4. **Gemini 3 Flash** (OpenRouter) — $0.50/$3, near-Pro reasoning, 1M context.
5. **GPT-5 Mini** (OpenRouter) — $0.25/$2, compact reasoning, 400K context.
6. **Devstral 2** (Chutes) — ~$0.05/$0.22, 123B agentic coding, 256K context.

### Best Frontier (When Quality is Everything)
1. **Claude Opus 4.6** (OpenRouter) — $5/$25, best reasoning, 1M context.
2. **GPT-5.2 Codex** (OpenRouter) — $1.75/$14, best agentic coding, 400K context.
3. **Gemini 3 Pro** (OpenRouter) — $2/$12, best multimodal, 1M context.
4. **Claude Sonnet 4.5** (OpenRouter) — $3/$15, excellent coding/agents, 1M context.

### Best Free
1. **Pony Alpha** (OpenRouter) — Stealth frontier model, Opus-class. Data logged.
2. **DeepSeek R1 0528** (OpenRouter) — Strong reasoning, 163K context.
3. **MiMo V2 Flash** (Chutes) — #1 open-source SWE-bench, 309B MoE.
4. **Devstral 2** (Chutes) — Free-tier agentic coding, 256K context.

## Tips

1. **Start with Chutes models** — they're your best value via subscription
2. **Use Copilot for frontier work** — flat rate crushes pay-per-token economics
3. **Pony Alpha is the free frontier wildcard** — Opus-class at $0, but data is logged. Never for Kato/work.
4. **DeepSeek V3.2 is the value king** — GPT-5 class at 1/50th the cost
5. **Use OpenRouter to spend credits** — model testing, comparisons, experimentation
6. **Run model comparisons** by orchestrating 3+ subagents on the same task
7. **Use Gemini 3 Flash** when you need 1M context at a reasonable price
8. **Reserve Claude Opus 4.6** for truly complex architecture/reasoning tasks
9. **Use free models** (Pony Alpha, DeepSeek R1, MiMo) for experimentation and drafts
