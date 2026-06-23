# ai-rules

A rule library and agentic workflow system for building software with AI. This repo provides:

- **Cursor rules** (`.mdc`) for the Developer agent — micro-level coding standards
- **Gem prompts** for PO, Designer, and PM agents — macro-level phase orchestration
- **Document templates** for structured handoffs between agents
- **PM architecture constraints** so technical plans respect layer boundaries before code is written

Human-in-the-loop gating at every phase. Git and PR review between tasks.

---

## Agentic Development Workflow

Four agents operate in sequence. Each agent owns a phase. Artifacts pass forward. No agent skips ahead or absorbs another agent's responsibilities.

```
Idea → PO Agent → Designer Agent → PM Agent → Dev Agent (Cursor)
         ↓              ↓              ↓              ↓
       prd.md      wireframes      ADR/SDD/      code + PR
                     mockups        tasks.md
```

### Phase 1 — Concept & Design (PO & Designer)

| Agent | Role | Output |
|-------|------|--------|
| **PO Agent** | Defines *what* and *why*. No tech. | `project.md`, `prd.md` |
| **Designer Agent** | Defines layout and visual structure. No production code. | `mockups/` wireframes (HTML/SVG) |

**PO Agent** (`docs/gems/po-agent.md`)
- Interrogates the idea with 2–3 scope questions before writing anything
- Generates `prd.md` only on explicit command ("Generate PRD")
- Stays business-focused — no databases, APIs, or libraries

**Designer Agent** (`docs/gems/designer-agent.md`)
- Reads PRD + design vision
- Asks 1–2 layout questions, summarizes structure
- Generates wireframe HTML/SVG only on explicit command ("Generate", "Approved")

**Artifacts produced:**

| File | Purpose |
|------|---------|
| `project.md` | Lightweight project anchor — status, stack summary, doc index, progress log |
| `prd.md` | Product requirements — features, user stories, acceptance criteria |
| `mockups/` | Static wireframes — layout, hierarchy, component placement |

---

### Phase 2 — Technical Planning (PM Agent)

| Agent | Role | Output |
|-------|------|--------|
| **PM Agent** | Translates product + design into technical blueprints. No production code. | `ADR.md`, `sdd.md`, `tasks.md` |

**Inputs:** `prd.md`, wireframes, `stack.md`, `docs/pm-fe-rules.md`, `docs/pm-be-rules.md`

**PM Agent** (`docs/gems/pm-agent.md`)
- Asks 2–3 technical questions (schema, API payloads, state flow) before generating
- Generates all three artifacts only on explicit approval ("Generate", "Approved")

**Artifacts produced:**

| File | Purpose |
|------|---------|
| `ADR.md` | Architecture Decision Records — key choices, trade-offs, alternatives |
| `sdd.md` | System Design Document — schema, API contracts, data flow, layer map |
| `tasks.md` | Sequential developer checklist — one task at a time, explicit action items |

PM tasks must follow the layer boundaries in `docs/pm-fe-rules.md` and `docs/pm-be-rules.md`.

---

### Phase 3 — Development (Dev Agent / Cursor)

| Agent | Role | Output |
|-------|------|--------|
| **Dev Agent** | Executes `tasks.md` action items. No architectural decisions. | Code + PR |

**Inputs:** `project.md`, `sdd.md`, active task from `tasks.md`, relevant `.mdc` rules from `rules/`

**Execution loop:**
1. Read `tasks.md`. Pick the next open task.
2. Complete all action items for that task only.
3. Submit code as a PR.
4. Human reviews, tests, merges.
5. Human authorizes the next task.

The Dev agent does not skip tasks, combine tasks, or redesign architecture mid-execution.

---

## Repository Structure

```
ai-rules/
├── docs/
│   ├── gems/                  # Agent system prompts (PO, Designer, PM)
│   │   ├── po-agent.md
│   │   ├── designer-agent.md
│   │   └── pm-agent.md
│   ├── pm-fe-rules.md         # Frontend architecture constraints for PM
│   ├── pm-be-rules.md         # Backend architecture constraints for PM
│   └── templates/             # Artifact templates copied into projects
│       ├── project.md         # Project anchor and doc index
│       ├── prd.md             # Product Requirements Document
│       ├── stack.md           # Tech stack constraints
│       ├── sdd.md             # System Design Document
│       ├── adr.md             # Architecture Decision Record
│       └── tasks.md           # Developer task checklist
│
└── rules/                     # Cursor .mdc rules for the Dev agent
    ├── global/                # Always-on core behavior
    ├── languages/             # JS, TS, Python, PHP
    ├── frontend/              # Vue, React, Next, Nuxt, Pinia, Zustand, etc.
    ├── backend/               # Node, Express, REST, database
    ├── infra/                 # Netlify, Supabase, Expo, CI/CD
    ├── practices/             # Cross-cutting: security, testing, migrations, etc.
    └── testing/               # Jest, Vitest
```

---

## Document Templates

Copy `docs/templates/` into a new project and fill them in as agents produce artifacts.

| Template | Created by | Used by |
|----------|-----------|---------|
| `project.md` | PO / human | All agents — project anchor |
| `prd.md` | PO Agent | Designer, PM |
| `stack.md` | Human | PM, Dev |
| `sdd.md` | PM Agent | Dev |
| `adr.md` | PM Agent | Dev, future PM |
| `tasks.md` | PM Agent | Dev |

---

## Cursor Rules (Dev Agent)

Rules live in `rules/` as `.mdc` files with YAML frontmatter.

| Category | Activation | Purpose |
|----------|-----------|---------|
| `global/` | `alwaysApply: true` | Core behavior, communication |
| `languages/`, `frontend/`, `backend/`, `infra/`, `testing/` | File globs | Stack-specific standards |
| `practices/` | Keywords + on-demand | Security, migrations, testing philosophy, etc. |

### Using rules in a project

1. Copy relevant `.mdc` files from `rules/` into the project's `.cursor/rules/`.
2. Fill in `stack.md` — this tells agents which rules apply.
3. Keep `global/core.mdc` always on. Add stack rules matching your tech.
4. Add `practices/` rules as needed — keyword-triggered, not always-on, to save context tokens.

```
project/
├── .cursor/rules/          # Copied from ai-rules/rules/
├── docs/
│   ├── project.md
│   ├── prd.md
│   ├── stack.md
│   ├── sdd.md
│   ├── adr.md
│   └── tasks.md
└── mockups/
```

---

## Agent Responsibility Matrix

| Concern | PO | Designer | PM | Dev |
|---------|:--:|:--------:|:--:|:---:|
| Business requirements | ✓ | | | |
| User stories | ✓ | | | |
| Wireframes / layout | | ✓ | | |
| Tech stack selection | | | | human |
| Architecture decisions | | | ✓ | |
| API / schema design | | | ✓ | |
| Task breakdown | | | ✓ | |
| Code implementation | | | | ✓ |
| PR / review gating | | | | human |

---

## Getting Started

### New project

1. Copy `docs/templates/project.md` and `docs/templates/stack.md` into your project.
2. Fill in `stack.md` with your chosen technologies.
3. Configure Gem agents using prompts from `docs/gems/`.
4. Run the workflow: PO → Designer → PM → Dev.
5. Copy applicable rules from `rules/` into `.cursor/rules/`.

### Existing project

1. Fill in `stack.md` to document current stack.
2. Copy matching rules from `rules/` into `.cursor/rules/`.
3. Use PM templates to generate `sdd.md` and `tasks.md` for the next feature.

---

## License

MIT — see [LICENSE](LICENSE).
