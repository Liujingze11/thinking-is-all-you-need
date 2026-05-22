---
name: thinking-is-all-you-need
description: Use when the task requires multi-step planning, has interdependent constraints, involves error-prone structured output (code, JSON, config), or the model is instructed to think carefully before answering. Especially effective for smaller models that benefit from explicit reasoning structure.
---

# Thinking Is All You Need

## Overview

Force structured step-by-step reasoning before producing a final answer. The four-phase process catches mistakes, contradictions, and omissions that surface-level answers miss.

**For LLMs, Attention Is All You Need. For AI Agents, Thinking Is All You Need.**

LLMs need attention to process tokens. Agents need structured thinking to plan, reason, and verify before acting. This skill enforces that methodology, especially critical for smaller models where implicit reasoning is weak.

**Core principle:** An answer is only as good as the thinking behind it. Explicit beats implicit.

## When to Use

- Multi-step planning (architecture, workflow design, algorithm selection)
- Error-prone structured output (code, config, JSON, workflows)
- Tasks with mutually constraining requirements
- User explicitly asks for careful reasoning or "想清楚再回答"
- **Using a smaller/weaker model** and need to boost reasoning quality

**When NOT to use:**
- Simple factual queries ("What does git status do?")
- Trivial single-step tasks
- Tasks the model already handles reliably
- Casual conversation or chitchat

## The Four Phases

Copy this checklist and check off each phase as you complete it:

```
Reasoning Progress:
- [ ] Phase 1: Deconstruct — all requirements, constraints, edge cases listed
- [ ] Phase 2: Plan — every output unit enumerated
- [ ] Phase 3: Trace — all paths walked, gaps found and fixed
- [ ] Phase 4: Verify — all Phase 1 constraints checked and passed
- [ ] Ready for final output
```

Complete each phase before moving to the next. Output phases with clear markers.

### Phase 1: Deconstruct

Break the request into atomic requirements. List every constraint, dependency, and edge case. No summarization. No "etc." or "...".

```
## Phase 1: Deconstruct

Requirements:
1. [specific requirement]
2. [specific requirement]

Constraints:
- [hard constraint]
- [soft constraint / preference]

Edge cases:
- [what if X is empty?]
- [what if Y conflicts with Z?]
```

### Phase 2: Plan

Enumerate every unit of output. For code: every function/module. For text: every section. For structured data: every record/field. Show the complete skeleton pre-filled where possible.

```
## Phase 2: Plan

Output units:
1. [unit name] — [what it does / what it contains]
2. [unit name] — [what it does / what it contains]
```

### Phase 3: Trace

Walk through the plan on every possible path. For code: trace execution. For workflows: trace every branch. For arguments: verify every claim has support.

Flag any gap, contradiction, or uncertainty. Fix the plan before proceeding.

```
## Phase 3: Trace

Path A (happy path): [walk through] → OK
Path B (error case): [walk through] → Found gap: [describe]
Path C (edge case): [walk through] → OK

Fixes applied to plan:
- [what changed]
```

### Phase 4: Verify

Check the output against every constraint from Phase 1. Check format rules. Check for duplicates, omissions, violations. Correct everything before final output.

```
## Phase 4: Verify

Constraint check:
- [constraint 1]: PASS / FAIL → [fix if needed]
- [constraint 2]: PASS / FAIL → [fix if needed]

Format check: PASS / FAIL
Completeness check: PASS / FAIL
```

## Final Output

After all four phases pass verification, output `---正式输出---` followed by the clean result. No explanations, no reasoning fragments, no Markdown wrapping unless the task requires it.

## Red Flags — STOP and Return to Phase 1

- "This is too simple to need reasoning" → Simple tasks hide subtle edge cases
- "I already thought about this" → Implicit reasoning is unreliable; make it explicit
- Skipping Trace because "the plan looks correct" → Untraced plans contain hidden contradictions
- Verifying without checking against Phase 1 constraints → Verification without a checklist is rubber-stamping
- Rushing to output because "the answer is obvious" → Obvious answers are where unexamined assumptions live
- Writing final output while skipping any phase → Incomplete reasoning = incomplete answer

## Anti-Rationalization Table

| Excuse | Reality |
|--------|---------|
| "This task is straightforward" | Straightforward tasks have unexamined assumptions. 30 seconds of reasoning saves 5 minutes of rework. |
| "I can reason implicitly" | Small models in particular benefit from explicit structure. Even large models miss details when reasoning is invisible. |
| "The phases take too long" | Wrong answers take longer. Four phases cost tokens, bad output costs trust. |
| "I'll do Phase 3 and 4 together" | Trace finds problems; Verify checks fixes. Combining them means unverified fixes. |
| "One quick answer this time" | Every "this time" becomes the pattern. Consistency matters. |

## Example (Illustrative)

> This demonstrates the format and depth expected. Actual output varies by model and task.

**Request:** "Generate a Docker Compose file for a Node.js app with PostgreSQL and Redis, with health checks."

### Phase 1: Deconstruct

```
Requirements:
1. Node.js app service with build context
2. PostgreSQL service with persistent volume
3. Redis service
4. Health checks on all three services
5. Correct dependency order (app depends on db + redis)

Constraints:
- PostgreSQL needs named volume for data persistence
- Health checks must use appropriate commands (pg_isready, redis-cli ping, curl)
- App must wait for healthy dependencies before starting

Edge cases:
- What if PostgreSQL data directory doesn't exist on first run?
- What port conflicts might occur?
```

### Phase 2: Plan

```
Output units:
1. docker-compose.yml — version, services (app, db, redis), volumes, networks
   - app: build ., ports, environment, depends_on with condition, healthcheck
   - db: image, volumes, environment, healthcheck
   - redis: image, healthcheck
```

### Phase 3: Trace

```
Path A (first run): db creates data dir → redis starts → both healthy → app starts → OK
Path B (subsequent run): db mounts existing volume → OK
Path C (db slow start): depends_on with condition: service_healthy prevents premature app start → OK
Path D (port conflict): using standard ports (3000, 5432, 6379); may need to parameterize → flag

Found gap: No .env.example or port configuration mechanism → add comment in compose file
```

### Phase 4: Verify

```
Constraint check:
- Node.js app service: PASS
- PostgreSQL with persistent volume: PASS (pgdata volume defined)
- Redis service: PASS
- Health checks on all services: PASS
- Correct dependency order: PASS (depends_on with service_healthy)

Format check: PASS (valid YAML)
Completeness check: PASS
```

---正式输出---
```yaml
version: "3.8"
services:
  app:
    build: .
    ports:
      - "${APP_PORT:-3000}:3000"
    environment:
      DATABASE_URL: postgresql://${POSTGRES_USER:-app}:${POSTGRES_PASSWORD:-secret}@db:5432/${POSTGRES_DB:-app}
      REDIS_URL: redis://redis:6379
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 10s
      timeout: 5s
      retries: 5

  db:
    image: postgres:16-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-app}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-secret}
      POSTGRES_DB: ${POSTGRES_DB:-app}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER:-app}"]
      interval: 5s
      timeout: 3s
      retries: 5

  redis:
    image: redis:7-alpine
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 5

volumes:
  pgdata:
```

## Model-Specific Notes

- **Haiku / small models**: This process is most impactful here. Smaller models produce more reliable results with explicit structure than without.
- **Sonnet**: Use for non-trivial tasks where correctness matters more than speed.
- **Opus / large reasoning models**: Skip for routine tasks; use for complex multi-constraint problems where even strong models can miss interactions.

## Extraction

If the caller only needs the final output, extract everything after `---正式输出---` and discard the reasoning phases. This marker is the contract between reasoning and output.
