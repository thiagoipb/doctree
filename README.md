# doctree

A set of skills for creating and maintaining documentation in AI software engineering. These skills are used to prepare the groundwork for foundational documentation, create specs, and maintain the organization and alignment of documents. This set comprises the [OPES-IA](https://github.com/thiagoipb/OPES-IA) program's utility suite and has been made available for community use.

## Skills

doctree provides 5 skills that cover the full documentation lifecycle of a software project:

| # | Skill | Purpose | When to use |
|---|-------|---------|-------------|
| 1 | [project-preview](skills/project-preview/SKILL.md) | Market research and business vision | Starting a new project and need to validate the idea |
| 2 | [doc-foundation](skills/doc-foundation/SKILL.md) | Canonical docs production (PRD, Architecture, etc.) | Project needs its foundational documentation base |
| 3 | [spec-corpus](skills/spec-corpus/SKILL.md) | Operational specs production | Project needs detailed specs for each area |
| 4 | [doc-maintainer](skills/doc-maintainer/SKILL.md) | Doc maintenance, audit, and correction | Docs need fixes, updates, coherence audits, or pruning |
| 5 | [project-recover](skills/project-recover/SKILL.md) | Code vs docs alignment verification | Code and documentation may have drifted apart |

## Lifecycle

The skills are designed to be used in sequence, though each can also be used independently:

```
project-preview  ──>  doc-foundation  ──>  spec-corpus
  (research)           (canonical docs)     (operational specs)
                              |                    |
                              v                    v
                       doc-maintainer  <──>  project-recover
                        (maintenance)        (alignment check)
```

**Starting a new project from scratch:**
`project-preview` → `doc-foundation` → `spec-corpus`

**Project already has code but no docs:**
`doc-foundation` → `spec-corpus`

**Docs exist but may be outdated:**
`project-recover` → `doc-maintainer`

**Routine maintenance:**
`doc-maintainer` (standalone)

## Installation

### Claude Code

Copy the skill directories into your Claude Code skills folder:

```bash
# Clone the repo
git clone https://github.com/thiagoipb/doctree.git

# Copy all skills
cp -r doctree/skills/* ~/.claude/skills/
```

Or copy individual skills:

```bash
# Copy only the skills you need
cp -r doctree/skills/doc-foundation ~/.claude/skills/
cp -r doctree/skills/spec-corpus ~/.claude/skills/
```

### Manual

Each skill is a single `SKILL.md` file. You can also copy them manually into your `~/.claude/skills/` directory, each in its own folder.

## What each skill produces

| Skill | Artifacts |
|-------|-----------|
| project-preview | `docs/preview/Market-Research.md`, `docs/preview/Business-Vision.md` |
| doc-foundation | 9 canonical docs in `docs/` (Glossary, PRD, Milestones, Architecture, Data-and-API, UX-Flows, Design-System, Maintenance, README) |
| spec-corpus | Operational specs in `docs/specs/`, status index, production plan |
| doc-maintainer | Maintenance reports, canonical change records, audit reports, pruning reports |
| project-recover | `docs/recovery/ALIGNMENT-REPORT-YYYY-MM-DD.md` |

## Skill details

### project-preview

Researches and validates a project idea before any documentation is written. Uses multi-agent research with adversarial verification to produce well-grounded market analysis and business vision.

**Key features:**
- 4 parallel research subagents (market analyst, competitor scout, audience researcher, opportunity finder)
- Adversarial verification of high-stakes claims
- Persona stress-testing through simulated interviews
- Dual-perspective business vision (strategic optimist + pragmatic skeptic)
- Go/No-Go viability checkpoint

### doc-foundation

Produces the 9 canonical documents that define a software project's product, architecture, experience, and process layers.

**Key features:**
- Two production modes: all at once or one by one
- Two detail levels: compact (operational) or expanded (with business context)
- Adaptive scope: detects existing docs and asks what to produce
- Automated validator for cross-references and completeness
- Glossary-first approach: shared language before any other doc

### spec-corpus

Produces a validated corpus of operational specifications covering every area of a project.

**Key features:**
- 5-phase process: Survey → Plan → Infrastructure → Production → Delivery
- Standard Production Procedure (PPS) with 7 steps per spec
- Automated validator with trap awareness
- Reconciliation tracking for doc-code divergences
- Batch commit strategy

### doc-maintainer

Maintains the canonical documentation base through 5 operation modes.

**Key features:**
- Two autonomy levels: routine (autonomous) vs canonical (approval required)
- 5 modes: routine maintenance, canonical change, coherence audit, log pruning, pending treatment
- 6 health tests for systematic cross-checking
- Cascade handling for changes that impact multiple docs
- ADR immutability enforcement

### project-recover

Verifies alignment between code and documentation across three dimensions of drift.

**Key features:**
- Three drift dimensions: Docs→Code, Code→Docs, Docs↔Code State
- Evidence standard: every finding cites both doc source and code evidence
- Severity classification (critical / important / minor)
- Co-decision model: user decides treat / discard / defer per finding
- Recovery plan with routing recommendations

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI, desktop app, or IDE extension)
- For `project-preview`: web search capability (subagents use WebSearch/WebFetch)

## License

[MIT](LICENSE)
