---
name: doc-maintainer
description: Use when a project's canonical docs need maintenance — fixing typos and formatting, applying approved changes to canonical docs, auditing coherence between docs, pruning old logs, or treating pending doc issues. Triggers on "maintain docs", "fix docs", "audit doc coherence", "prune logs", "doc health check", or any request to keep project documentation accurate and consistent.
---

# Maintaining the Canonical Doc Base

Ongoing maintenance of a project's foundational documentation: corrections, canonical changes, coherence audits, log pruning, and pending issue treatment.

**Announce at start:** "I'm using the doc-maintainer skill to maintain the canonical documentation."

## Overview

Canonical docs are living documents — they drift, accumulate inconsistencies, and need regular care. This skill operates in 5 modes, each with its own scope, autonomy level, and artifact. The user chooses the mode at session start.

The governing principle is **two levels of autonomy**:
- **Routine** (form): changes that improve HOW a doc expresses what it already decides. Autonomous with report.
- **Canonical** (decision): changes that alter WHAT a doc decides or asserts. Diff + explicit approval before applying.

**The dividing line:** if the change alters what the doc decides or asserts → canonical (approval). If it only improves how the doc expresses what it already decides → routine (autonomous with report).

## Session Start

Every invocation begins by establishing the session mode:

```
What type of maintenance session?

1 — Routine maintenance
    (form corrections — typos, formatting, links, nomenclature; autonomous with report)

2 — Canonical change
    (applying an approved change that alters meaning/decisions in a canonical doc)

3 — Coherence audit
    (cross-check between canonical docs — docs vs docs)

4 — Log pruning
    (cleanup of operational log directories)

5 — Pending doc treatment
    (treating a doc issue from an audit, a recovery process, or a user decision)
```

If the user already signals the mode in their request, proceed directly.

## Mode 1 — Routine Maintenance

**Scope:** Form corrections that do NOT alter decisions recorded in docs.

Examples: typos, grammar, inconsistent formatting, broken links, stale cross-references (e.g., "see section 3" when it moved to section 4), terminology clearly divergent from Glossary in non-decisional text.

**Autonomy:** Autonomous with report. Apply corrections, then report what was done. No prior approval needed per correction.

**Critical limit:** If a form correction turns out to touch a decision, STOP and reclassify as Mode 2. When in doubt whether something is form or decision, treat it as decision (approval).

### Flow

1. Read the canonical docs in scope
2. Identify form corrections needed
3. Apply corrections
4. Produce **Routine Maintenance Report**
5. Commit with descriptive message

### Routine Maintenance Report

```markdown
# Routine Maintenance Report — [date]

**Scope**: [docs reviewed]

## Corrections applied

| Doc | Location | Type | Before → After |
|-----|----------|------|----------------|
| [doc] | [section/line] | [typo/format/link/reference/nomenclature] | [summary] |

## Reclassifications

[If any correction was reclassified to Mode 2, list here]

## Commits

- [hash + descriptive message]
```

## Mode 2 — Canonical Change

**Scope:** Applying a change that alters the meaning or decision in a canonical doc.

The change can originate from:
- A direct user decision
- An audit finding (Mode 3) that surfaced an inconsistency requiring a decision
- A planning process or review that produced a change recommendation
- A pending issue (Mode 5) that requires updating a doc's decisions

**Autonomy:** Always requires approval. Present the diff (current state → proposed state) and wait for explicit approval before applying.

### Flow

1. Read the change source (user request, audit report, planning artifact)
2. Read the target canonical doc in its current state
3. Prepare the change proposal (diff: current → proposed)
4. **Present the diff to the user** — wait for approval
5. If approved, apply the change
6. **If the change is in Architecture:** create a formal ADR. If it supersedes a previous ADR, mark the previous as "Superseded by ADR-XXXX"
7. **If the change cascades** (e.g., PRD change impacts Milestones and Architecture): identify all affected docs, present the full cascade diff, apply all changes together after approval
8. Commit with descriptive message referencing the change source
9. Produce **Canonical Change Record**

### Cascade Handling

Changes to foundational docs (PRD, Architecture) often cascade to downstream docs. The dependency chain from `doc-foundation` applies:

```
Glossary → PRD → Milestones → Architecture → Data-and-API
                                PRD → UX-Flows → Design-System
                                              All → Maintenance → README
```

When a change touches an upstream doc:
1. Trace downstream: which docs reference or depend on the changed content?
2. For each affected doc, assess: does it need a corresponding update?
3. Present ALL necessary changes as a single cascade diff
4. Apply atomically — all or nothing, after approval

### Canonical Change Record

```markdown
# Canonical Change Record — [date]

**Doc(s) changed**: [list]
**Origin**: [user decision / audit finding / planning artifact]
**User approval**: [confirmed]

## Change applied

[Summary: previous state → new state]

## ADR (if Architecture)

- ADR created: [ADR-NNNN-title]
- Supersedes: [ADR-XXXX, if applicable — marked as superseded]

## Cascade (if applicable)

[List all docs updated as part of the cascade, with summary of each change]

## Commits

- [hash + descriptive message]
```

## Mode 3 — Coherence Audit

**Scope:** Verify internal coherence BETWEEN canonical docs. This is docs-vs-docs, not code-vs-docs (code verification is `project-recover` territory).

### Health Tests

Run these checks systematically. Each produces a pass/fail with evidence.

| # | Test | Pass criterion |
|---|------|----------------|
| H1 | **Cross-reference integrity** | All ADRs referenced in Architecture; all internal doc links resolve |
| H2 | **Glossary coverage** | No domain term used in docs without a Glossary entry |
| H3 | **Milestones currency** | Milestones reflects the actual project state (verify with user) |
| H4 | **PRD–Architecture alignment** | Architectural decisions are consistent with product principles |
| H5 | **Design System consistency** | Design-System doc matches implementation tokens (if code exists) |
| H6 | **Data model consistency** | Data-and-API doc matches actual schema/contracts (if code exists) |

For H5 and H6: if no code exists yet, skip — these only apply when implementation has started. When code exists, read the relevant files and cite what was consulted.

### Additional cross-checks

- PRD vs Milestones: do phases reflect what the product promises?
- ADRs between themselves: no active contradictions; supersessions correctly marked
- UX-Flows vs PRD: flows cover specified features
- Glossary used consistently across all docs (same terms, same meanings)

**Autonomy:** Identify and report only — do NOT auto-correct. Findings become proposals:
- Form incoherences → recommend Mode 1 session
- Decision incoherences → recommend Mode 2 session
- Structural issues → recommend user evaluation

### Flow

1. Read all canonical docs in scope (full reads, not skimming)
2. For assertions about code, read the code and cite files consulted
3. Execute health tests and cross-checks
4. Produce **Coherence Audit Report**
5. Classify each finding: form (treatable in Mode 1) or decision (requires Mode 2)
6. Present to user — do NOT apply corrections in this session

### Coherence Audit Report

```markdown
# Coherence Audit Report — [date]

**Scope**: [docs audited]
**Code consulted** (if applicable): [files read]

## Summary

- Total inconsistencies found: [N]
- Form (treatable in Mode 1): [X]
- Decision (require Mode 2): [Y]

## Health Test Results

| Test | Result | Notes |
|------|--------|-------|
| H1 — Cross-references | PASS/FAIL | [details] |
| H2 — Glossary coverage | PASS/FAIL | [details] |
| H3 — Milestones currency | PASS/FAIL | [details] |
| H4 — PRD–Architecture | PASS/FAIL | [details] |
| H5 — Design System | PASS/FAIL/SKIP | [details] |
| H6 — Data model | PASS/FAIL/SKIP | [details] |

## Inconsistencies Found

### Inconsistency 1 — [title]

- **Docs involved**: [doc A vs doc B]
- **Description**: [what is inconsistent, factual]
- **Nature**: [form | decision]
- **Recommended treatment**: [Mode 1 / Mode 2 / user evaluation]

## Recommended Next Sessions

[Which maintenance sessions the user should consider]
```

## Mode 4 — Log Pruning

**Scope:** Cleanup of operational log/archive directories that grow over time.

This is NOT about canonical docs — it's about operational files (build logs, agent reports, execution histories, etc.).

### Flow

1. Ask the user which directories to prune and what retention criteria to apply
2. If the project has a Maintenance doc with pruning rules, use those as defaults
3. List files that are candidates for pruning (by age, count, redundancy)
4. For sensitive directories (history, decisions): present candidates and wait for approval before removing
5. For operational logs: apply pruning per criteria, reporting what was removed
6. Produce **Pruning Report**

### Pruning Report

```markdown
# Pruning Report — [date]

**Criteria applied**: [retention rules used]

## Removed

| Directory | Files removed | Criterion |
|-----------|--------------|-----------|
| [dir] | [N files, period] | [age/count/redundancy] |

## Preserved

[What was kept and why — especially in sensitive directories]
```

## Mode 5 — Pending Doc Treatment

**Scope:** Treat doc issues that arrived from external sources:
- A coherence audit (Mode 3) that found issues needing treatment
- A `project-recover` session that surfaced doc-level issues
- A user decision to adjust a doc (e.g., after a code review revealed a doc gap)

**Autonomy:** Depends on the nature — form issues are treated as Mode 1 (autonomous); decision issues as Mode 2 (approval). The issue usually arrives with the decision already made, so the focus is faithful materialization.

### Flow

1. Read the source of the pending issue (audit report, recovery artifact, user description)
2. Read the target canonical doc
3. Classify: form or decision
4. If decision: present diff, wait for approval. If form: apply and report
5. If it involves Architecture: create ADR per Mode 2 rules
6. Commit and produce the appropriate record (Mode 1 or Mode 2 format)

## Inviolable Principles

1. **Preserve the author's writing style.** Your role is consistency and currency, not rewriting with your own voice. The doc base has the author's personality — protect it, don't replace it.

2. **Read code before asserting things about it.** In any audit or proposal involving code state, read the relevant files and cite what was consulted. Never infer code state without verification.

3. **ADRs are immutable.** Never edit an existing ADR. When an architectural decision changes, create a new ADR that supersedes the previous one (mark previous as "Superseded by ADR-XXXX").

4. **Materialize, don't decide.** Product and architectural decisions come from the user. You apply changes faithfully — you don't reinterpret them.

5. **When in doubt, escalate.** If you're unsure whether something is form or decision, treat it as decision (require approval). The cost of asking is low; the cost of unauthorized change is high.

## Red Flags — STOP

- Applying a canonical change without presenting the diff first
- Editing an existing ADR instead of creating a new one
- Auto-correcting findings from an audit (Mode 3 identifies; treatment is a separate session)
- Rewriting doc content in your own style
- Making product or architecture decisions
- Pruning without explicit session request (Mode 4)
- Changing code — this skill maintains docs, not code

## Integration

| Context | Related skill |
|---------|--------------|
| Produces the docs this skill maintains | `doc-foundation` |
| May surface doc issues for treatment | `project-recover` |
| Doc changes may trigger spec updates | `spec-corpus` |
| Research artifacts may inform changes | `project-preview` |
