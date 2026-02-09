# Agent Configuration Guide

## Architecture

- **2 full pantheons** via oh-my-opencode-slim presets (Personal + Kato)
- **1 primary agent** — Pony Alpha (FREE frontier)
- **7 testing subagents** — frontier OpenRouter models

## Personal Pantheon (Default)

Cost-optimized. Configured in `oh-my-opencode-slim.json` → `agents`.

| Agent | Role | Model | Provider | Cost |
|-------|------|-------|----------|------|
| **orchestrator** | Master delegator | Kimi K2.5 | NanoGPT | $0.60/$3 |
| **explorer** | Codebase search | GPT-5.1-Codex-Mini | Copilot | 0.33x |
| **librarian** | Docs research | Gemini 3 Flash | Copilot | 0.33x |
| **oracle** | Architecture, debugging | Pony Alpha | OpenRouter | FREE |
| **designer** | UI/UX design | Kimi K2.5 | NanoGPT | $0.60/$3 |
| **fixer** | Fast implementation | GPT-5.1-Codex-Mini | Copilot | 0.33x |

## Kato Pantheon (Premium)

For client work. Configured in `oh-my-opencode-slim.json` → `presets.kato`.

| Agent | Role | Model | Provider | Cost |
|-------|------|-------|----------|------|
| **orchestrator** | Master delegator | GPT-5.2 Codex | Copilot | 1x |
| **explorer** | Codebase search | GPT-5.1-Codex-Mini | Copilot | 0.33x |
| **librarian** | Docs research | Gemini 3 Pro | Copilot | 1x |
| **oracle** | Architecture, debugging | Claude Sonnet 4.5 | Copilot | 1x |
| **designer** | UI/UX design | Claude Sonnet 4.5 | Copilot | 1x |
| **fixer** | Fast implementation | GPT-5.2 Codex | Copilot | 1x |

Activate: `OH_MY_OPENCODE_SLIM_PRESET=kato opencode`

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
| `oh-my-opencode-slim.json` | Personal + Kato pantheon model overrides |

## NanoGPT Provider

NanoGPT provides cost-effective alternatives to Chutes with better uptime.

| Model | Use Case | Cost (per 1M) |
|-------|----------|---------------|
| `moonshotai/kimi-k2.5` | Coding, orchestration, design | $0.60/$3 |
| `moonshotai/kimi-k2.5:thinking` | Reasoning tasks | $0.60/$3 |
| `deepseek/deepseek-v3.2` | GPT-5 class reasoning | $0.40/$1.20 |
| `deepseek/deepseek-v3.2:thinking` | Deep reasoning | $0.40/$1.20 |
| `deepseek/deepseek-r1` | Reasoning specialist | $0.40/$1.20 |
| `zai-org/glm-4.7` | Fast, cost-effective | $1/$2 |

## Copilot Multiplier Reference

| Multiplier | Models |
|-----------|--------|
| **0x** | GPT-4.1, GPT-4o, GPT-5 mini, Raptor mini |
| **0.25x** | Grok Code Fast 1 |
| **0.33x** | Claude Haiku 4.5, Gemini 3 Flash, GPT-5.1-Codex-Mini |
| **1x** | Sonnet 4/4.5, Gemini 3 Pro, GPT-5/5.1/5.2/Codex |
| **3x** | Claude Opus 4.5/4.6 — avoid |
| **10x** | Claude Opus 4.1 — never |
