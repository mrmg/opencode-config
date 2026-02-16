# Agent Configuration Guide

## Architecture

- **Dual-model strategy** — MiniMax M2.5 (coding) + GLM-5 (reasoning)
- **1 primary agent** — Pony Alpha (FREE frontier)
- **7 testing subagents** — frontier OpenRouter models

## Model Distribution

Balanced across MiniMax M2.5 and GLM-5 based on strengths:

### MiniMax M2.5 (Coding Workhorse)
- 80.2% SWE-Bench Verified
- 76.8% BFCL Tool Calling
- 50-100 TPS (Lightning)
- FREE (1000 prompts/5hr)

### GLM-5 (Reasoning Specialist)
- 92.7% AIME Math
- 77.8% SWE-Bench
- 744B params, 40B active
- 3x cost multiplier currently

### GLM-4.6V (Vision)
- 128K context
- Native multimodal tool calling
- Only vision option

## Agents

| Agent | Model | Role | Strength Used |
|-------|-------|------|---------------|
| **sisyphus** | MiniMax M2.5 | Primary coder | 80.2% SWE, tool-calling |
| **hephaestus** | GLM-5 | Builder | Reasoning depth |
| **oracle** | GLM-5 | Debugging | Deep reasoning |
| **librarian** | MiniMax M2.5 | Docs research | Speed, tool-calling |
| **explore** | GLM-5 | Codebase search | Deep understanding |
| **multimodal-looker** | GLM-4.6V | Vision | Multimodal |
| **prometheus** | GLM-5 | Planning | Reasoning |
| **metis** | MiniMax M2.5 | Pre-planning | Fast analysis |
| **momus** | GLM-5 | Review | Critical thinking |
| **atlas** | Kimi K2.5 | Fallback | Chutes availability |

**Distribution: 4 MiniMax, 5 GLM-5, 1 GLM-4.6V, 1 Kimi**

## Categories

| Category | Model | Rationale |
|----------|-------|-----------|
| **visual-engineering** | MiniMax M2.5 | Better coding benchmark |
| **ultrabrain** | GLM-5 xhigh | Hard logic needs reasoning |
| **deep** | GLM-5 medium | Deep understanding |
| **artistry** | MiniMax M2.5 | Creative coding |
| **quick** | MiniMax M2.5 | Fast execution |
| **unspecified-low** | GLM-5 | Spread load |
| **unspecified-high** | GLM-5 high | Needs reasoning |
| **writing** | MiniMax M2.5 | Docs don't need deep reasoning |

**Distribution: 4 MiniMax, 4 GLM-5**

## Primary Agent

| Agent | Model | Provider | Cost |
|-------|-------|----------|------|
| `pony` | Pony Alpha | OpenRouter | FREE |

Switch to it: `/use pony`

**Warning:** Data is logged for training — personal use only.

## Testing Subagents

Frontier models for comparison. Spin up from orchestrator:
`Use test-opus subagent to review this code`

| Agent | Model | Price (per 1M) |
|-------|-------|---------------|
| `test-opus` | Claude Opus 4.6 | $5/$25 |
| `test-sonnet` | Claude Sonnet 4.5 | $3/$15 |
| `test-codex` | GPT-5.2 Codex | $1.75/$14 |
| `test-gpt52` | GPT-5.2 | $1.75/$14 |
| `test-gemini-pro` | Gemini 3 Pro | $2/$12 |
| `test-grok4` | Grok 4 | varies |
| `test-pony` | Pony Alpha | FREE (data logged) |

## Config Files

| File | Purpose |
|------|---------|
| `opencode.jsonc` | Pony primary, testing subagents, NanoGPT provider, MCPs, plugins |
| `oh-my-opencode.json` | Agent model assignments (MiniMax + GLM distribution) |

## Fallback: Chutes

When MiniMax/GLM rate limits hit, Chutes provides alternatives:

| Model | Use Case |
|-------|----------|
| `chutes/zai-org/GLM-5-TEE` | GLM-5 fallback |
| `chutes/MiniMaxAI/MiniMax-M2.5-TEE` | MiniMax fallback |
| `chutes/moonshotai/Kimi-K2.5-TEE` | Kimi fallback |
| `chutes/deepseek-ai/DeepSeek-V3.2-TEE` | DeepSeek reasoning |

## Usage Guidelines

### When to use MiniMax M2.5
- Primary coding tasks
- Fast implementation (quick category)
- Tool-calling heavy workflows
- Documentation writing
- UI/frontend work

### When to use GLM-5
- Complex debugging (oracle)
- Architecture planning (prometheus)
- Code review (momus)
- Deep analysis (deep, ultrabrain)
- Mathematical/algorithmic work

### When to use GLM-4.6V
- Screenshot analysis
- Diagram interpretation
- PDF reading
- Visual debugging
