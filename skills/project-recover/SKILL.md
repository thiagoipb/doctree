---
name: project-recover
description: Use when a project's code and documentation may have drifted apart — features implemented but not documented, features documented but not built, or docs contradicting each other about implementation state. Triggers on "check alignment", "code vs docs", "verify project state", "technical leveling", "find drift", "project recovery", or any request to compare what the docs say with what the code actually does.
---

# Verifying Project Alignment: Code vs Docs

Systematic comparison between what the documentation says the project is/does and what the code actually implements. Identifies drift, presents findings, and produces a recovery plan with user decisions.

**Announce at start:** "I'm using the project-recover skill to verify alignment between code and documentation."

## Overview

Projects drift. Code evolves faster than docs. Decisions get made in code but never reach the docs. Docs describe features that were never built, or were built differently. This skill detects that drift across three dimensions and produces a recovery plan.

The governing posture is **investigative, not corrective** — this skill identifies and organizes drift; it does not fix it. Fixes are routed to the appropriate skill or process after the user decides what to treat.

## The Three Dimensions of Drift

| Dimension | What it compares | What drift looks like |
|-----------|-----------------|----------------------|
| **Docs → Code** | Documented features vs actual implementation | PRD says "badge system" but code has no badge logic |
| **Code → Docs** | Implemented features vs documentation coverage | Code has a notification system that no doc mentions |
| **Docs ↔ Code State** | What docs say about implementation state vs reality | Milestones marks "auth complete" but auth routes are stubs |

All three dimensions work on the same axis: **verifying against docs**. The question is always "does reality match what the docs say?"

## Phase 1: Scope Definition

**Goal:** Define what to verify and how deep to go.

Present scope choices:

**Verification scope:**
> What should I verify?
> (a) **Full project** — compare all canonical docs against the full codebase
> (b) **Specific area** — focus on one domain (e.g., auth, data model, UX flows)
> (c) **Specific doc** — verify one canonical doc against its corresponding code

**Depth:**
> How thorough?
> (1) **Surface scan** — check major features, entities, and contracts. Fast, catches obvious drift.
> (2) **Deep verification** — read code in detail, trace flows, verify edge cases. Thorough, catches subtle drift.

If the user already signals scope in their request, proceed directly.

## Phase 2: Survey

**Goal:** Build a complete picture of both sides — docs and code.

### Read the docs (what SHOULD exist)

Read canonical docs relevant to the scope, in order:
1. PRD — what features and behaviors are promised
2. Milestones — what is marked as complete vs planned
3. Architecture — what technical decisions were made
4. Data-and-API — what entities, contracts, and policies are defined
5. UX-Flows — what user journeys are specified
6. Design-System — what visual standards are defined
7. Glossary — what terms and concepts are established
8. Specs (if they exist) — operational specifications per area

For each doc, extract **verifiable claims** — concrete statements about what exists, how it works, what it includes. These become the checklist for comparison.

### Read the code (what DOES exist)

Read the codebase relevant to the scope:
1. Directory structure — what modules/features exist
2. Schema/data model — what entities are actually defined
3. Routes/endpoints/actions — what API surface exists
4. Components — what UI elements are built
5. Configuration — what services, integrations, and tools are configured
6. Tests — what behavior is verified by tests

For each area, extract **implementation facts** — concrete evidence of what is built, how it works, what it includes.

### Cross-reference

Build a comparison matrix: for each verifiable claim from docs, is there corresponding code? For each implementation fact from code, is there corresponding documentation?

## Phase 3: Drift Detection

**Goal:** Systematically identify every point of drift.

### Dimension 1 — Docs → Code (documented but not implemented)

For each verifiable claim from the docs:
- Is there corresponding code? (yes/no/partial)
- If partial: what is implemented and what is missing?
- Severity: **critical** (core feature missing), **important** (secondary feature missing), **minor** (detail differs)

### Dimension 2 — Code → Docs (implemented but not documented)

For each implementation fact from the code:
- Is there corresponding documentation? (yes/no/partial)
- If no: is this an intentional feature, a prototype, or dead code?
- Severity: **critical** (major undocumented system), **important** (significant undocumented feature), **minor** (small undocumented detail)

### Dimension 3 — Docs ↔ Code State (docs misrepresent implementation state)

For each doc that makes state claims (e.g., "completed", "active", "planned"):
- Does the code match the claimed state?
- Common patterns: Milestones says "done" but code is stubbed; Architecture says "uses X" but code uses Y; Data-and-API documents endpoints that don't exist

Severity: **critical** (doc fundamentally misrepresents state), **important** (partial misrepresentation), **minor** (outdated detail)

### Evidence standard

Every drift finding MUST include evidence from both sides:
- **Doc side:** file path, section, and the specific claim (`docs/PRD.md §3.2: "gamification with badges"`)
- **Code side:** file path, line number or directory, and what was found or not found (`src/features/ — no badge-related module exists`)

No drift finding without evidence from both sides. If you cannot cite the source, you have not verified it.

## Phase 4: Discussion

**Goal:** Present findings to the user for co-decision on each item.

Present drift findings organized by dimension, then by severity (critical first). For each finding:

1. State the drift clearly — what the doc says vs what the code shows
2. Cite evidence from both sides
3. Assess routing — where should the fix happen:
   - **Doc needs updating** → `doc-maintainer` (the code is right, the doc is stale)
   - **Code needs implementing** → development work (the doc is right, the code is missing)
   - **Decision needed** → the drift reveals an unresolved decision (neither side is clearly "right")
4. The user decides per item: **treat**, **discard**, or **defer**

Do NOT decide discards on your own. A drift that looks trivial might be a symptom of a deeper issue. Present everything; the user filters.

## Phase 5: Recovery Plan

**Goal:** Produce an artifact with all findings and user decisions.

### Recovery Plan artifact

Save as `docs/recovery/ALIGNMENT-REPORT-YYYY-MM-DD.md` (or equivalent location per project convention).

```markdown
# Alignment Report — [date]

**Scope**: [full project / area / specific doc]
**Depth**: [surface scan / deep verification]
**Docs consulted**: [list with paths]
**Code consulted**: [list with paths]

## Summary

| Dimension | Critical | Important | Minor | Total |
|-----------|----------|-----------|-------|-------|
| Docs → Code | [n] | [n] | [n] | [n] |
| Code → Docs | [n] | [n] | [n] | [n] |
| Docs ↔ Code State | [n] | [n] | [n] | [n] |
| **Total** | [n] | [n] | [n] | [N] |

## Findings

### Dimension 1 — Documented but Not Implemented

#### Finding 1.1 — [title]

- **Severity**: [critical / important / minor]
- **Doc says**: [claim] — `[file:section]`
- **Code shows**: [reality] — `[file:line]` or `[directory — absent]`
- **Routing**: [doc-maintainer / development / decision needed]
- **User decision**: [treat → route X / discard (reason) / defer]

### Dimension 2 — Implemented but Not Documented

#### Finding 2.1 — [title]
[same format]

### Dimension 3 — Docs Misrepresent State

#### Finding 3.1 — [title]
[same format]

## Recovery Routing

### Items to treat

| # | Finding | Route to | Action |
|---|---------|----------|--------|
| 1.1 | [title] | doc-maintainer | Update [doc] to reflect [reality] |
| 2.3 | [title] | development | Implement [feature] per [doc spec] |

### Items deferred

| # | Finding | Reason | Revisit when |
|---|---------|--------|-------------|

### Items discarded

| # | Finding | Reason |
|---|---------|--------|
```

## Inviolable Principles

1. **Investigate, don't correct.** This skill identifies drift and produces a recovery plan. It does NOT modify docs (that's `doc-maintainer`), write code (that's development), or update specs (that's `spec-corpus`). It investigates and delivers findings.

2. **Evidence from both sides.** Every finding must cite the doc source AND the code evidence. "I think this feature is missing" is not a finding. `docs/PRD.md §4: "real-time notifications" — src/features/ has no notification module` is a finding.

3. **Don't decide discards alone.** Present all drift to the user. What looks trivial may be critical. What looks critical may be intentional. The user has context you don't.

4. **Respect scope boundaries.** Verify what the user asked to verify. A full-project scan is expensive — don't expand a focused scope without asking.

5. **State, don't judge.** Report drift factually. "PRD says X, code shows Y" — not "the code is wrong" or "the doc is outdated." Which side is correct is a decision for the user.

## Red Flags — STOP

- Modifying any doc or code file (this skill is read-only + artifact)
- Reporting drift without evidence from both sides (doc AND code citation)
- Deciding that a drift item is "not worth mentioning" without presenting to user
- Expanding scope beyond what user defined
- Treating docs-vs-docs coherence issues (that's `doc-maintainer` Mode 3 territory — unless the incoherence is specifically about implementation state claims)

## Integration

| Context | Related skill |
|---------|--------------|
| Drift in docs → fix docs | `doc-maintainer` (Mode 2 or Mode 5) |
| Missing docs entirely | `doc-foundation` (produce canonical docs) |
| Specs need update after alignment | `spec-corpus` (produce/update operational specs) |
| Research before recovery | `project-preview` (understand market/business context) |
