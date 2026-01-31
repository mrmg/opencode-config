# Agent Configuration Guide

This document describes how to use the opencode agent system with the orchestrator pattern.

## Quick Start

**Default Agent:** `kato-coder` (Claude Sonnet 4.5)

**To switch agents:**
```
/use kato-architect
/use personal-coder
/use ollama-private
```

**To use a subagent via orchestrator:**
```
Use gpt-5.2 subagent to analyze this complex problem
Use gemini-3-flash subagent to search the codebase
```

## Primary Agents

### Work (Kato) - Best Models (via GitHub Copilot & Chutes)

| Agent | Model | Purpose | Best For |
|-------|-------|---------|----------|
| `kato-architect` | Claude Opus 4.5 (Copilot) | Architecture & Design | System design, complex refactoring, technical decisions |
| `kato-coder` | Claude Sonnet 4.5 (Copilot) | Implementation | Daily coding tasks, feature implementation |
| `kato-strategist` | Claude Opus 4.5 (Copilot) | Strategic Planning | Planning, analysis, complex problem solving |
| `kato-typescript` | **Kimi K2.5 (Chutes)** | TypeScript Specialist | TypeScript/React projects, rivals Claude Opus 4.5, cheaper |

### Personal - Mixed Models

| Agent | Model | Purpose | Best For |
|-------|-------|---------|----------|
| `personal-architect` | GPT-5.2 | Architecture & Design | System design, prototyping |
| `personal-coder` | Claude Sonnet 4.5 | Implementation | Side projects, personal coding |
| `personal-strategist` | GPT-5.2 | Strategic Planning | Research, analysis, planning |

### Special Purpose

| Agent | Model | Purpose | Best For |
|-------|-------|---------|----------|
| `orchestrator` | GPT-5.2 | Subagent Management | Delegating tasks to specific models |
| `ollama-private` | GLM-4.7 Flash GGUF | Private Local | Sensitive code, offline work |
| `ollama-fast` | Ministral 3 14B | Fast Local | Quick local tasks, prototyping |
| `free-primary` | GLM-4.7 | Fallback Primary | When paid models unavailable |
| `free-rapid` | Grok Code | Fast Fallback | Quick tasks, high token rate (296 tok/s) |

## Subagent Mappings

Use these with the orchestrator: "Use `<subagent>` subagent to <task>"

### By Task Type

| Task | Recommended Subagent | Alternative |
|------|---------------------|-------------|
| **Security review** | `security-reviewer` (gpt-5.2) | `claude-opus-4-5` |
| **Release notes** | `release-notes` (gemini-3-pro) | `gpt-5.2` |
| **Documentation** | `documenter` (gemini-3-flash) | `claude-sonnet-4-5` |
| **Code review** | `reviewer` (kimi-k2-5) | `claude-sonnet-4-5` |
| **Research/librarian** | `librarian` (gemini-3-flash) | `gpt-5-nano` |
| **Codebase exploration** | `explorer` (gemini-3-flash) | `glm-4.7` |
| **Frontend/UI/UX** | `frontend` (kimi-k2-5) | `gemini-3-flash` |
| **Test generation** | `tester` (kimi-k2-5) | `gpt-5.2-codex` |
| **Performance optimization** | `optimizer` (gpt-5.2-codex) | `claude-opus-4-5` |
| **QA planning** | `qa` (glm-4.7) | `gpt-5.1` |
| **Refactoring** | `refactorer` (kimi-k2-5) | `claude-sonnet-4-5` |
| **Multimodal analysis** | `multimodal` (gemini-3-pro) | - |
| **Quick/simple tasks** | `rapid` (gpt-5-nano) | `gemini-3-flash` |
| **TypeScript work** | `typescript-specialist` (kimi-k2-5) | `claude-opus-4-5` |

### By Model Capability

#### Claude (Anthropic) - via GitHub Copilot
- `claude-opus-4-5` - Most capable, best for complex tasks (80.9% SWE-bench)
- `claude-sonnet-4-5` - Balanced performance and speed (77.2% SWE-bench)
- `claude-sonnet-4` - Previous generation, still capable
- `claude-opus-4-1` - Specialized variant
- `claude-haiku-4-5` - Fast, lightweight
- `claude-3-5-haiku` - Previous generation fast model

#### GPT (OpenAI) - via GitHub Copilot
- `gpt-5.2` - Most capable GPT model (80% SWE-bench)
- `gpt-5.2-codex` - Specialized for coding
- `gpt-5.1` - Strong general performance
- `gpt-5.1-codex` - Coding optimized
- `gpt-5.1-codex-max` - Maximum context coding
- `gpt-5.1-codex-mini` - Lightweight coding
- `gpt-5` - General purpose
- `gpt-5-codex` - Basic coding
- `gpt-5-nano` - Fastest, cheapest

#### Gemini (Google) - via Chutes
- `gemini-3-pro` - Most capable Gemini
- `gemini-3-flash` - Fast and efficient (78% SWE-bench)

#### Kimi (Moonshot AI) - via Chutes
- `kimi-k2-5` - **NEW: Kimi K2.5** - Excellent for TypeScript, rivals Claude Opus 4.5, more cost-effective
- `kimi-2.5` - Moonshot AI
- `kimi-k2` - Alternative Moonshot model

#### Others - via Chutes
- `glm-4.7` - Strong performance, free tier (73.8% SWE-bench, 95.7% AIME)
- `glm-4.6` - Lightweight GLM
- `minimax-m2.1` - MiniMax model
- `qwen3-coder` - Alibaba coding model
- `big-pickle` - OpenCode alternative

## Usage Examples

### Example 1: Complex Architecture Decision
```
/use kato-architect
I need to redesign our authentication system to support OAuth2, SAML, and SSO.
What architecture would you recommend?
```

### Example 2: Using Specialist Subagent for Security Review
```
/use orchestrator
Use security-reviewer subagent to perform a security review of the auth module.
Focus on: input validation, SQL injection, XSS vulnerabilities.
```

### Example 3: Documentation Generation
```
/use orchestrator
Use documenter subagent to generate API documentation for the user endpoints
in src/routes/user.ts. Create OpenAPI-style docs.
```

### Example 3b: TypeScript Refactoring
```
/use orchestrator
Use typescript-specialist subagent to refactor this React component to use
proper TypeScript patterns with strict typing.
```

### Example 4: Personal Project
```
/use personal-coder
Help me build a Python script that scrapes Hacker News and saves to SQLite.
```

### Example 5: Private/Sensitive Work
```
/use ollama-private
Review this internal configuration file for any exposed secrets.
```

### Example 6: Fast Fallback
```
/use free-rapid
Quickly summarize the changes in the last 5 commits.
```

## Temperature Settings

All agents use optimized temperatures:
- **0.0-0.2**: Coding, reasoning, analysis (deterministic)
- **0.3-0.5**: Documentation, creative tasks
- **0.7+**: UI/UX design, highly creative work

## Configuration Files

- **Main config**: `~/.config/opencode/opencode.jsonc`
- **This guide**: `~/.config/opencode/AGENTS.md`

## Tips

1. **Start with kato-coder** for most work tasks
2. **Use kato-typescript** for TypeScript/React work (Kimi K2.5 rivals Claude Opus 4.5, cheaper!)
3. **Use specialist subagents** for specific tasks (e.g., `reviewer`, `tester`, `refactorer` all use Kimi K2.5 for TypeScript)
4. **Use orchestrator + subagents** for complex multi-step tasks
5. **Prefer Gemini 3 Flash** for fast research/exploration (cheaper, fast)
6. **Use Claude Opus 4.5** for critical architecture decisions
7. **Fallback to free agents** when paid models hit limits
8. **Use Ollama agents** for sensitive/private code

## Model Selection Flowchart

```
Is it work-related?
├── Yes → Use kato-* agents
│   ├── Architecture → kato-architect (Opus 4.5)
│   ├── TypeScript/React → kato-typescript (Kimi K2.5) ⭐
│   ├── Daily coding → kato-coder (Sonnet 4.5)
│   └── Strategy → kato-strategist (Opus 4.5)
└── No → Personal project?
    ├── Yes → Use personal-* agents
    └── Sensitive/private?
        ├── Yes → ollama-private
        └── No → free-rapid for quick tasks

Need specific model capability?
→ Use orchestrator: "Use <model> subagent to <task>"
```
