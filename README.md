# Thinking Is All You Need

[![agentskills.io](https://img.shields.io/badge/agentskills.io-compatible-4B32C3)](https://agentskills.io)

> *Inspired by "Attention Is All You Need" (Vaswani et al., 2017) & "If I had an hour to solve a problem, I'd spend 55 minutes thinking and 5 minutes solving." — Albert Einstein*

A **model-agnostic reasoning skill** that forces structured thinking before producing answers.

**For LLMs, Attention Is All You Need. For AI Agents, Thinking Is All You Need.**

If Transformers proved attention is the architecture, this skill proves thinking is the methodology.

> 📖 **Read the story behind this project:** [How Adding a "Thinking Workflow" to My Agent Suddenly Boosted Accuracy](STORY.md) — English & 中文

## Problem

Large reasoning models think internally. Smaller models skip straight to output, producing:

- Code with logic errors from unexamined constraints
- JSON/config missing fields or contradicting itself
- Plans that break on the first edge case

## Solution

**Thinking Is All You Need** adds a four-phase reasoning gate before any answer:

```
Deconstruct → Plan → Trace → Verify → Output
```

Each phase catches what the previous missed. The `---正式输出---` marker separates the thinking from the answer — ship the output, strip the reasoning.

## Quick Example

**Without Thinking** (one-shot):

```yaml
# "Docker Compose for Node.js + PostgreSQL + Redis"
services:
  app:
    build: .
    ports: ["3000:3000"]
    depends_on: [db, redis]
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
  redis:
    image: redis:7
```

Missing: health checks, named volumes, `depends_on` conditions, `.env` parameterization — six oversights.

**With Thinking Is All You Need** — catches all six in Phase 3 (Trace), fixes in Phase 4 (Verify), outputs production-ready config.

## The Four Phases

| Phase | What Happens | Output |
|-------|-------------|--------|
| **Deconstruct** | Break into atomic requirements, constraints, edge cases | Exhaustive checklist |
| **Plan** | Enumerate every output unit | Full skeleton |
| **Trace** | Walk every execution path; flag gaps | Fixed plan |
| **Verify** | Check against Phase 1 constraints | Validated output |

Then: `---正式输出---` → clean, correct answer.

## Installation

```bash
# Claude Code
mkdir -p ~/.claude/skills/thinking-is-all-you-need
cp SKILL.md ~/.claude/skills/thinking-is-all-you-need/
```

## When to Use

| Scenario | Use? |
|----------|------|
| Code with interdependent constraints | Yes |
| Structured output (JSON, YAML, config) | Yes |
| Multi-step architecture / workflow design | Yes |
| Smaller/weaker models (Haiku, etc.) | Yes — most impactful here |
| Simple factual query | No |
| Casual conversation | No |

## Model Effectiveness

| Model | Without | With | Boost |
|-------|---------|------|-------|
| Haiku | Baseline errors | Significant improvement | ★★★★★ |
| Sonnet | Occasional oversight | Catches edge cases | ★★★ |
| Opus | Already strong | Useful for complex tasks | ★ |

**The smaller the model, the bigger the gain.** Explicit structure compensates for weaker implicit reasoning.

## Name

A tribute to two ideas:

1. **"Attention Is All You Need"** — the 2017 paper that changed NLP forever. Attention is how LLMs process information; **Thinking is how AI Agents should act on it.**
2. **Einstein's 55/5 rule** — spend 55 minutes thinking, 5 minutes acting. This skill enforces that ratio for agents.

## Comparison

| Skill | Scope | Style |
|-------|-------|-------|
| **Thinking Is All You Need** | Universal — any task | 4-phase internal monologue |
| Systematic Debugging | Bugs only | Domain-specific tracing |
| Brainstorming | Creative design | User collaboration |
| Verification Before Completion | Claims | Command-based checks |

The only skill that gates **every answer type** behind a reasoning process with machine-extractable output.

## File Structure

```
thinking-is-all-you-need/
├── SKILL.md    # The skill
├── STORY.md    # The origin story (English & 中文)
├── README.md   # This file
└── LICENSE     # MIT
```

## License

MIT — use freely, modify, distribute.
