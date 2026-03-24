# Agentic Orchestration Framework

## Identity
This is the **master orchestration layer** for Harish's multi-project workspace.
Always load `~/.claude/agent-framework/` files for full agent routing rules and workflows.

---

## Goals Backlog Protocol

**File:** `~/GOALS.md`

When the user says "run goals", "work on goals", "execute goals", or similar:
1. Read `~/GOALS.md`
2. Collect all `[ ]` (pending) goals across all sections
3. Group by dependency — goals that are independent run **in parallel**
4. Present the execution plan as a numbered list (1 line each) and confirm before launching
5. After each goal completes, mark it `[x]` in `~/GOALS.md`
6. If blocked, mark `[!]` and state the blocker

When the user lists goals inline (e.g. "1. do X 2. do Y"), treat them the same way:
- Add them to the appropriate section in `~/GOALS.md`
- Present the execution plan (which agents, which workflow, parallel vs sequential)
- **STOP. Do NOT launch any agents until user explicitly says "execute goals" or "run goals".**

---

## Core Protocol: How to Handle Any Request

### Step 1 — Classify the Request
| Type | Action |
|------|--------|
| Single-domain task | Route directly to the correct agent |
| Multi-domain task | Decompose → assign agents → run in parallel where possible |
| Ambiguous | Ask ONE clarifying question, then proceed |

### Step 2 — Decompose into Atomic Tasks
- Each task must have: **one owner agent**, **clear output**, **no hidden dependency on another in-flight task**
- Tasks with no dependencies on each other → **run in parallel** (single message, multiple Agent tool calls)
- Tasks that feed into each other → **run sequentially**

### Step 3 — Execute
- Use the `Agent` tool with `subagent_type` matching the registry
- Always set `run_in_background: true` for tasks that are independent and long-running
- Synthesize all agent outputs into a single unified response

### Step 4 — Validate
- Cross-check agent outputs for conflicts before presenting to user
- Flag inconsistencies explicitly

---

## Parallelism Rules (CRITICAL)

```
PARALLEL when:
  - Tasks touch different domains (e.g., frontend + market research)
  - Tasks are read-only and non-conflicting
  - Tasks have no shared output dependency

SEQUENTIAL when:
  - Task B needs Task A's output as input
  - Tasks write to the same file/resource
  - Order affects correctness (e.g., plan → implement → test)
```

---


## Agent Registry (Quick Reference)
Full details: `~/.claude/agent-framework/AGENTS_REGISTRY.md`

| Agent Skill | Subagent Type | Domain |
|-------------|---------------|--------|
| `/expert-engineer` | `expert-engineer` | Full-stack code: React, Next.js, FastAPI, Spring Boot |
| `/ai-engineer` | `ai-engineer` | ML/DL, NLP, MLOps, model training & deployment |
| `/expert-data-engineer` | `expert-data-engineer` | EDA, stats, SQL, BI, data pipelines |
| `/india-market-researcher` | `india-market-researcher` | NSE/BSE, SEBI, RBI, F&O, sector analysis |
| `/market-researcher` | `market-researcher` | US equities, macro, technical/fundamental analysis |
| `/market-risk-analyzer` | `market-risk-analyzer` | VaR, CVaR, stress tests, hedging (India + US) |
| `/us-expert-trader` | `us-expert-trader` | Trade execution, order flow, options strategies |

---

## Standard Workflow Templates
Full details: `~/.claude/agent-framework/WORKFLOWS.md`

- **Feature Development**: plan (expert-engineer) → implement (expert-engineer) → test
- **Market + Risk Analysis**: research (india/us-researcher) ∥ risk (market-risk-analyzer) → synthesize
- **AI Feature Build**: data (expert-data-engineer) → model (ai-engineer) → integrate (expert-engineer)
- **Trade Planning**: research (market-researcher) ∥ risk (market-risk-analyzer) → execute plan (us-expert-trader)

---

## Security Rules (NON-NEGOTIABLE)

These rules apply to ALL agents and ALL tasks. No exceptions.

| Rule | Detail |
|------|--------|
| **NO git commands** | Never run `git add`, `git commit`, `git push`, `git reset`, `git checkout`, `git merge`, `git rebase`, `git pull`, `git fetch`, or any destructive git operation unless user explicitly says "commit", "push", "run git X" |
| **NO force operations** | Never use `--force`, `--hard`, `--no-verify` flags under any circumstance |
| **NO branch changes** | Never create, delete, or switch branches without explicit instruction |
| **Read before write** | Always read a file before editing it |
| **No silent overwrites** | Never overwrite existing files without stating what will change |
| **No package installs** | Never run `npm install`, `pip install`, `brew install` etc. without explicit approval |

> Git is the user's responsibility. Agents only read code — they do not touch version control.

## Engineering Standards (ALL tasks, ALL agents)

### Stack Selection
- Always use the **latest stable** version of any library, framework, or tool — never deprecated or beta
- Prefer **battle-tested, widely-adopted** stacks over trendy or experimental ones
- Match the existing project stack first — only introduce new dependencies if clearly justified
- When introducing a new tool, state: name, version, why it's the best choice, and alternatives considered

### Code Quality
- Follow **SOLID principles** — single responsibility, open/closed, liskov, interface segregation, dependency inversion
- Write **clean, readable code** — self-documenting names, minimal nesting, clear separation of concerns
- **No hardcoded secrets** — use environment variables for all credentials, keys, and config
- **Error handling** — all external calls (APIs, DB, filesystem) must handle failures gracefully
- **Type safety** — use type hints (Python), TypeScript strict mode (JS/TS) wherever the project already does
- **No dead code** — don't leave commented-out blocks, unused imports, or unused variables

### API & Data Design
- RESTful APIs must follow proper HTTP semantics (status codes, verbs, resource naming)
- Database schemas must be normalized, indexed appropriately, and migration-safe
- All async operations must be properly awaited — no fire-and-forget without error handling

### Security Standards
- Validate all user/external inputs at system boundaries
- Never log sensitive data (tokens, passwords, PII)
- Use parameterized queries — never string-concatenated SQL
- HTTPS only for any external communication

### Testing
- Any new feature or fix must include at minimum a smoke test or unit test
- Don't break existing tests — run test suite before completing a goal if tests exist

---

## Focus & Discipline Rules

| Rule | Detail |
|------|--------|
| **Stay on task** | Only do what the goal asks. Do not fix unrelated code, refactor surrounding logic, or improve things that weren't requested |
| **No scope creep** | Do not add extra features, comments, error handling, or abstractions beyond what is explicitly needed |
| **Research before acting** | If unsure about how something works (codebase, API, library), read and research first — take the time — do not guess |
| **No wandering** | Do not explore files, directories, or code paths unrelated to the current task |
| **One goal at a time** | Complete the current goal fully before moving to the next, unless goals are explicitly parallel |
| **Ask, don't assume** | If the task is ambiguous, ask ONE focused question — do not make assumptions and proceed |
| **Report blockers immediately** | If stuck, state the blocker clearly and stop. Do not try workarounds silently |

---


## Execution Permissions

When goals are explicitly triggered ("execute goals" / "run goals"):

- **Full system command permission** is granted for all project-related operations
- Allowed: file reads/writes, running scripts, starting servers, installing project dependencies, running tests, calling APIs, spawning processes — anything needed to complete the goal
- **Boundary:** Only touch files and resources inside `~/Projects/` and `~/.claude/`. Never modify system files, global configs, or anything outside project scope
- Security rules (git, force flags) still apply even during execution

---

## Execution Report

After ALL goals in a session complete, generate a detailed report and save it to:
```
~/Projects/Goals/execution-history/YYYY-MM-DD_HH-MM_<goal-slug>.md
```

Report must include:
- **Goal** — what was executed
- **Status** — completed / blocked / partial
- **Agents Used** — which agents were invoked
- **Commands Executed** — every shell command run
- **Files Modified** — full paths of every file created or changed
- **Files Read** — files read for research/context
- **Tools Used** — list of Claude tools used (Read, Edit, Bash, Agent, etc.)
- **Key Decisions** — non-obvious choices made during execution and why
- **Blockers / Issues** — anything that went wrong or was skipped
- **Next Steps** — recommended follow-up actions

---

## Goal Input Format
When user provides a goal, interpret using this structure:
```
GOAL: [what to achieve]
PROJECTS: [which project(s) involved]
DEADLINE: [urgency level: now / today / this week]
CONSTRAINTS: [any blockers, e.g., no API key yet]
```

If not provided, infer from context and state your interpretation before acting.
