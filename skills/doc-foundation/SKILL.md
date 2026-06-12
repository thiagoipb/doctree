---
name: doc-foundation
description: Use when a project needs its canonical documentation base — PRD, Milestones, Architecture, Data-and-API, UX-Flows, Design-System, Glossary, Maintenance, README. Triggers on "create project docs", "write canonical docs", "build the doc base", "document the project foundation", or any request to produce foundational documentation for a software project.
---

# Producing the Canonical Doc Base

End-to-end process for producing the foundational documentation of a software project: the 9 canonical docs that define product, architecture, experience, and process.

**Announce at start:** "I'm using the doc-foundation skill to produce the canonical documentation base."

## Overview

Canonical docs are **decision documents** — they encode what to build, how, and why. Each doc follows a template, references the others, and passes an automated validator. Production is co-creative: the user decides, the agent writes.

The process has 5 phases. The production order is fixed (each doc builds on the previous).

```
Intake → Glossary → Product → Technical → Experience+Process
```

## Production Order

| # | Doc | Depends on | Purpose |
|---|-----|-----------|---------|
| 1 | Glossary | Nothing | Shared language — terms, personas, domain concepts |
| 2 | PRD | Glossary | Product definition — who, what, why |
| 3 | Milestones | PRD | Roadmap — phases, waves, deliverables |
| 4 | Architecture | PRD, Milestones | Technical decisions — stack, constraints, diagrams |
| 5 | Data-and-API | Architecture | Data model, contracts, access policies |
| 6 | UX-Flows | PRD | User journeys — main flows, edge cases |
| 7 | Design-System | UX-Flows | Visual language — palette, typography, components |
| 8 | Maintenance | All above | Process — update triggers, health checks, pruning |
| 9 | README | All above | Entry point — executive summary for newcomers |

## Phase 1: Intake

**Goal:** Understand the project stage and configure the production run.

### Step 1 — Detect project stage

Read the project directory. Classify:

| Stage | Signal | Behavior |
|-------|--------|----------|
| New (no code) | Empty or scaffold-only repo | Produce all 9 docs |
| In progress (code + partial docs) | Some docs exist in `docs/` | Ask user: produce all, or only what's missing? |
| No docs at all (code exists) | Code but no `docs/` | Produce all 9 docs, using code as source of truth for as-is |

Do NOT rewrite existing docs — that is `doc-maintainer` territory. If docs exist and user wants them updated, recommend `doc-maintainer` instead.

### Step 2 — Check for preview artifacts

Look for `project-preview` outputs (Market Research, Business Vision) in the project. If found, use as input for Glossary and PRD. If not found, proceed without — the questionnaires will gather what's needed.

### Step 3 — User configuration

Present three choices before starting:

**Production mode:**
> How do you want the docs produced?
> (a) **All at once** — I write all docs, you review and adjust after.
> (b) **One by one** — I write each doc, deliver for your review, then proceed to the next.

**Detail level:**
> What level of detail?
> (1) **Compact** — shorter, objective, operational. Focused on decisions and structure.
> (2) **Expanded** — longer, with business context, technical explanations, and rationale in each section.

**Scope** (only if some docs exist):
> Some docs already exist: [list]. Produce all 9 (overwriting nothing — new files only), or only the missing ones?

### Step 4 — Create branch and infrastructure

1. Create branch (e.g., `docs/foundation`)
2. Create validator script `scripts/check-docs.mjs` (checks required sections per doc type, front-matter, prohibited placeholders, broken cross-references between docs)
3. Commit infrastructure

## Phase 2: Glossary

**Goal:** Establish shared language before writing anything else.

### Two-line approach

**Line 1 — Data-driven:** Extract terms from available sources:
- `project-preview` artifacts (market research, business vision)
- Existing docs (if any)
- Code (entity names, routes, components — if code exists)

**Line 2 — User-driven:** Short questionnaire:
- What is the domain of this project? (e.g., education, fintech, health)
- Who are the main user types/personas?
- What are the 5-10 most important terms someone needs to know to understand this project?
- Any terms that are used in a non-obvious way? (e.g., "quest" means a learning exercise, not a game quest)

**Output:** `docs/Glossary.md` — terms with definitions, organized by category.

**Gate:** User reviews Glossary. All subsequent docs use these terms consistently.

## Phase 3: Product Layer (PRD + Milestones)

### PRD

**Questionnaire (3-5 questions)**, cross-referencing preview artifacts:

1. What problem does this project solve? For whom? (cross with Market Research personas)
2. What are the 3-5 core features for MVP? (cross with Business Vision)
3. What does success look like? (metrics, adoption, revenue)
4. What is this project NOT? (non-goals — critical for scope control)
5. Any hard constraints? (budget, timeline, regulatory, platform)

**Output:** `docs/PRD.md`

### Milestones

Derived from the PRD — not a separate questionnaire. Structure:
- Phases/waves with clear deliverables
- Dependencies between phases
- Definition of Done per phase

**Output:** `docs/Milestones.md`

**Gate (mode b):** User reviews PRD + Milestones before technical decisions. In mode (b), ADRs are produced alongside Architecture for decisions made here.

## Phase 4: Technical Layer (Architecture + Data-and-API)

### Architecture

Based on PRD constraints and Milestones roadmap:
- Stack decisions (framework, database, hosting, etc.)
- System diagram (components, data flow)
- Key constraints (performance, security, scalability)
- Each significant decision becomes an ADR candidate (produced in mode b only)

**Output:** `docs/Architecture.md` + `docs/adr/ADR-NNNN-*.md` (mode b only)

### Data-and-API

Based on Architecture:
- Data model (entities, relationships)
- API contracts (endpoints or server actions)
- Access policies (RLS, roles, multi-tenancy if applicable)

**Output:** `docs/Data-and-API.md`

**Gate (mode b):** User reviews technical decisions.

## Phase 5: Experience + Process Layer

### UX-Flows

Based on PRD personas and features:
- Main user journeys (happy path)
- Key decision points and branches
- Error states and edge cases

**Output:** `docs/UX-Flows.md`

### Design-System

Based on UX-Flows:
- Color palette (with semantic tokens)
- Typography scale
- Core components (buttons, cards, inputs, navigation)
- Spacing and layout rules

**Output:** `docs/Design-System.md`

### Maintenance

Based on all previous docs:
- When to update each doc (triggers)
- Health check criteria
- Pruning rules for operational logs
- Who owns what (if team context is known)

**Output:** `docs/Maintenance.md`

### README

Executive summary synthesizing all docs:
- What the project is (from PRD)
- How to get started (from Architecture)
- Where to find what (links to all docs)
- Current status (from Milestones)

**Output:** `README.md` (project root)

**Gate (mode b):** User reviews experience and process docs.

## Validation

After all docs are produced:

1. **Validator pass** — run `scripts/check-docs.mjs docs/*.md`; expect all PASS
2. **Cross-reference check** — every doc-to-doc reference resolves; Glossary terms used consistently
3. **Completeness** — no placeholder sections, no empty templates
4. **Delivery summary** — present to user: docs produced, decisions captured, ADRs created (if mode b), recommended next step (`spec-corpus` for operational specs)

## Validator (`check-docs.mjs`)

Checks per doc type:
- Required sections present (each doc type has its own list)
- No prohibited placeholders (TBD, TODO, lorem ipsum — case-insensitive substring)
- Cross-references to other docs resolve (relative markdown links)
- Front-matter present (title, status, date, version)

## Detail Levels

| Section aspect | Compact | Expanded |
|----------------|---------|----------|
| Context/rationale | 1-2 sentences | 1-2 paragraphs with business context |
| Feature descriptions | Bullet list | Bullet list + explanation + user story |
| Technical decisions | Decision + stack chosen | Decision + alternatives considered + rationale |
| Diagrams | Text-based (ASCII/Mermaid) | Text-based with annotations |
| Examples | None or 1 | 2-3 per section |

## Red Flags — STOP

- Writing a doc before the Glossary exists
- Making architectural decisions without PRD approval
- Rewriting existing docs (recommend doc-maintainer instead)
- Producing ADRs in mode (a) (user hasn't reviewed yet)
- Using terms not defined in Glossary
- Skipping the user questionnaire and inferring product decisions

## Integration

| Context | Related skill |
|---------|--------------|
| Before doc-foundation | `project-preview` (produces research artifacts as input) |
| After doc-foundation | `spec-corpus` (produces operational specs from canonical docs) |
| Ongoing | `doc-maintainer` (maintains and updates the canonical base) |
| When drift detected | `project-recover` (realigns project with docs) |
