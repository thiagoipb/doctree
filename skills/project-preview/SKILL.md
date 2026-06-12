---
name: project-preview
description: Use when a project is starting and needs market research and business vision before writing canonical docs. Triggers on "research the market", "validate this idea", "project preview", "business vision", "market analysis", "is this idea viable", or any request to research and validate a software project idea before building documentation.
---

# Researching and Validating a Project Idea

End-to-end research and validation process for a software project idea: from initial concept through market research, adversarial verification, and persona stress-testing to two foundational artifacts.

**Announce at start:** "I'm using the project-preview skill to research and validate the project idea."

## Overview

Before writing canonical docs, a project needs grounding — who is the market, who are the competitors, what is the real opportunity, and what are the risks. This skill produces two artifacts that feed directly into `doc-foundation`:

| Artifact | Purpose | Feeds into |
|----------|---------|-----------|
| `Market-Research.md` | Market landscape, competitors, audience, trends | Glossary + PRD |
| `Business-Vision.md` | Value proposition, personas, differentials, features, risks | PRD + Milestones |

The process uses **multi-agent research** at 4 strategic points to produce findings that are thorough, adversarially verified, and stress-tested against simulated user objections.

## Phase 1: Intake

**Goal:** Understand the project idea and fill gaps through a structured questionnaire.

The user may arrive with anything from a one-sentence idea to a complete pitch. The questionnaire adapts — if the user already answered something in their description, don't repeat it.

### Questionnaire

1. **What is the project?** (free description — 1 sentence to 1 paragraph)
2. **For whom?** (target audience, initial personas if known)
3. **What problem does it solve?** (core pain point)
4. **Already know competitors?** (direct competitors, if any)
5. **Intended differential?** (why someone would use this instead of what already exists)
6. **Technical context** (existing code? team? what stage?)
7. **Business model** (if already considered — SaaS, freemium, marketplace, open-source, etc.)

Items the user doesn't know are marked as "to investigate" — the research phase will attempt to cover them.

### Configuration

**Research depth:**
> How thorough should the research be?
> (1) **Compact** — 3-5 competitors, key trends, core personas. Faster, lower token cost.
> (2) **Expanded** — 8-12 competitors, TAM/SAM/SOM, PESTEL analysis, detailed personas. Thorough, higher token cost.

**Language:**
> What language should the artifacts be written in?

## Phase 2: Parallel Research

**Goal:** Build a comprehensive market picture through 4 independent research lines.

Launch 4 subagents in parallel, each with a distinct investigation focus. Independence prevents cross-contamination — each line explores deeply without inheriting assumptions from the others.

### Subagent 1 — Market Analyst

**Focus:** Market size, trends, and macro forces.

Research:
- **TAM** (Total Addressable Market): if everyone with this problem paid, what's the revenue?
- **SAM** (Serviceable Addressable Market): what slice can this business model reach?
- **SOM** (Serviceable Obtainable Market): what's capturable in the first 1-2 years?
- Market growth trajectory (expanding, mature, declining)
- PESTEL analysis (political, economic, social, technological, environmental, legal forces) — in expanded mode
- Timing: is there a window of opportunity that may close?

**Output:** Market size report with numbers and sources.

### Subagent 2 — Competitor Scout

**Focus:** Exhaustive competitor mapping.

Research:
- **Direct competitors**: same problem, same solution approach
- **Indirect competitors**: same problem, different solution approach
- **Hidden substitutes**: what do people use today to get by (spreadsheets, WhatsApp groups, manual processes)
- For each competitor: value proposition, business model, pricing, strengths, weaknesses
- User complaints: what do reviews (app stores, forums, review sites) say about competitor weaknesses?

**Output:** Competitor list with analysis. Compact: 3-5 competitors. Expanded: 8-12 competitors.

### Subagent 3 — Audience Researcher

**Focus:** Target audience deep-dive.

Research:
- Demographic and behavioral profile of the target audience
- Core pains, frustrations, and obstacles related to the problem
- Current workarounds and their limitations
- Adoption barriers: what would prevent someone from trying a new solution?
- Decision criteria: what makes someone choose one solution over another?
- Discovery channels: where does this audience find new products?

**Output:** Audience profile with evidence from research.

### Subagent 4 — Opportunity Finder

**Focus:** Market gaps and differentiation possibilities.

Research:
- What do competitors do exceptionally well (table stakes — must match)?
- What do competitors do poorly or charge too much for (opportunity)?
- Underserved niches or segments the big players ignore
- Emerging technologies that enable new approaches
- Blue Ocean possibilities: where can you eliminate, reduce, raise, or create?

**Output:** Opportunity map with gaps, niches, and differentiation candidates.

## Phase 2.5: Adversarial Verification

**Goal:** Stress-test the most impactful findings before they enter the artifacts.

After the 4 research lines complete, identify the **high-stakes claims** — findings that, if wrong, would fundamentally mislead the project:

- Market size numbers (TAM/SAM/SOM)
- "No direct competitor exists in [market]" claims
- "The market is growing at X%" claims
- "Users' main pain is Y" claims
- Differential claims ("no one does Z")

Launch 1 subagent with a **skeptical mandate**:

> "You are a skeptical analyst. For each claim below, actively search for evidence that REFUTES it. Default to skepticism — if you cannot find strong counter-evidence, say so, but try hard. For each claim, report: confirmed (counter-evidence searched, not found), refuted (counter-evidence found — here it is), or inconclusive (mixed signals)."

### Processing results

| Verification result | Action |
|--------------------|--------|
| **Confirmed** | Enters artifact with high confidence marker |
| **Refuted** | Corrected in artifact with the counter-evidence |
| **Inconclusive** | Enters artifact marked as hypothesis, not fact |

## Phase 3: Synthesis + Persona Stress-Test

**Goal:** Synthesize research into personas and value proposition, then stress-test them.

### Step 1 — Build personas from research data

Using the Audience Researcher findings + Competitor Scout complaints + Opportunity Finder gaps, construct 2-3 personas:

Per persona:
- Fictional name and representative context
- Job-to-be-done (what they're trying to accomplish)
- Top 3 pains / Top 3 expected gains
- Current workaround and its limitations
- Main objections to adopting a new solution
- Discovery channels

### Step 2 — Draft value proposition and differentials

Based on the full research:
- Value proposition formula: "We help [persona] achieve [benefit] through [unique mechanism] without [main pain/objection]"
- Differentials: concrete, evidence-based (not aspirational)
- Positioning: where this sits on the competitive map

### Step 3 — Persona stress-test (subagent)

Launch 1 subagent that role-plays as each persona and confronts the value proposition:

> "You are [Persona A — description, context, pains, current workaround]. A new product proposes [value proposition]. Answer honestly:
> 1. Would you try this? Why or why not?
> 2. What would make you abandon your current workaround for this?
> 3. What would prevent you from adopting this?
> 4. How much would you pay? Why that amount?
> 5. What feature would make you open this app every day?
> Repeat for each persona."

### Processing stress-test results

- Objections that repeat across personas → highlight as critical risks
- Feature requests that emerge → feed into suggested features
- Pricing signals → inform business model section
- Adoption barriers → inform go-to-market considerations

## Phase 4: Dual Vision + Go/No-Go

**Goal:** Produce a balanced Business Vision from two perspectives, then present a viability checkpoint.

### Step 1 — Dual synthesis (2 subagents)

Launch 2 subagents with contrasting mandates:

**Subagent A — Strategic Optimist:**
> "Based on the research findings, write the opportunity case: market size, growth potential, differentiation opportunities, feature ideas that could create strong engagement, and why this project can succeed. Be specific — cite the research."

**Subagent B — Pragmatic Skeptic:**
> "Based on the research findings, write the risk case: market barriers, strong incumbents, weak differentials, challenging unit economics, adoption friction, and scenarios where this project fails. Be specific — cite the research."

The main agent synthesizes both perspectives into a balanced Business Vision — neither euphoric nor paralyzing.

### Step 2 — Suggested features

Based on the full research (competitor gaps, persona pains, stress-test results), organize feature suggestions:

| Category | Description |
|----------|-------------|
| **Core** (must-have) | Without these, the product doesn't function — the heart of the MVP |
| **Engagement** (retention) | What brings users back — notifications, progress tracking, community |
| **Differentiation** (competitive edge) | What competitors don't have that solves a latent pain |

### Step 3 — Go/No-Go checkpoint

Present a viability matrix to the user before finalizing artifacts:

| Criterion | Assessment | Evidence |
|-----------|-----------|----------|
| **Market viability** | Is the market large enough and growing? | [TAM/SAM/SOM data] |
| **Technical feasibility** | Can the core features be built with available resources? | [user's technical context] |
| **Monetization clarity** | Is it clear how the product generates revenue? | [business model + persona pricing signals] |
| **Usage frequency** | Does the problem occur often enough to sustain an app? | [audience research data] |
| **Sustainable differential** | Is there something hard to copy in the short term? | [opportunity finder data] |

The user reviews and decides: **proceed** (move to `doc-foundation`), **pivot** (adjust the concept and re-research specific areas), or **pause** (more investigation needed).

## Phase 5: Artifact Production + Delivery

**Goal:** Produce the two artifacts and deliver formally.

### Market-Research.md

Save to `docs/preview/Market-Research.md`:

```markdown
# Market Research — [Project Name]

**Date**: [YYYY-MM-DD]
**Research depth**: [compact / expanded]
**Sources consulted**: [list]

## 1. Market Size and Trajectory

[TAM/SAM/SOM with sources. Confidence level per number.]

## 2. Trends and Macro Forces

[Key trends. PESTEL summary if expanded mode.]

## 3. Competitive Landscape

### Competitor Matrix

| Competitor | Type | Value Proposition | Model | Price | Strengths | Weaknesses |
|-----------|------|-------------------|-------|-------|-----------|-----------|

### Hidden Substitutes

[What people use today without a formal product]

## 4. Target Audience

[Demographic and behavioral profile. Pains, workarounds, adoption barriers.]

## 5. Market Gaps and Opportunities

[Gaps identified. Underserved niches. Blue Ocean possibilities.]

## 6. Verification Status

| Claim | Status | Evidence |
|-------|--------|----------|
| [high-stakes claim] | confirmed / refuted / inconclusive | [source] |
```

### Business-Vision.md

Save to `docs/preview/Business-Vision.md`:

```markdown
# Business Vision — [Project Name]

**Date**: [YYYY-MM-DD]
**Based on**: Market-Research.md

## 1. Value Proposition

[Formula: We help [persona] achieve [benefit] through [mechanism] without [pain].]

## 2. Personas

### Persona 1 — [Name]
- Context and profile
- Job-to-be-done
- Top 3 pains / Top 3 expected gains
- Current workaround
- Main objections
- Stress-test results: [key findings from simulated interview]

### Persona 2 — [Name]
[same format]

## 3. Positioning and Differentials

[Where this sits on the competitive map. Evidence-based differentials.]

## 4. Suggested Features

### Core (MVP)
[Features without which the product doesn't function]

### Engagement (Retention)
[Features that bring users back]

### Differentiation (Competitive Edge)
[Features competitors don't have]

## 5. Business Model

[Model type. Pricing signals from persona stress-test. Revenue mechanics.]

## 6. Opportunity Case

[Strategic optimist perspective — with evidence]

## 7. Risk Case

[Pragmatic skeptic perspective — with evidence]

## 8. Go/No-Go Assessment

[Viability matrix results. User's decision.]
```

### Delivery

Present to user:
- Both artifacts with paths
- Summary of key findings (3-5 bullet points)
- Verification status of high-stakes claims
- Go/No-Go assessment
- Recommended next step: `doc-foundation` to produce canonical docs using these artifacts as input

## Red Flags — STOP

- Writing artifacts before the adversarial verification completes
- Presenting market size numbers without sources
- Treating inconclusive claims as confirmed facts
- Skipping the persona stress-test
- Deciding that the project is viable or not viable — that's the user's decision at the Go/No-Go checkpoint
- Producing canonical docs (PRD, Architecture, etc.) — that's `doc-foundation` territory

## Integration

| Context | Related skill |
|---------|--------------|
| After project-preview | `doc-foundation` (uses Market-Research + Business-Vision as input) |
| If docs already exist | Skip project-preview; go directly to `doc-foundation` or `project-recover` |
| For operational specs | `spec-corpus` (after doc-foundation) |
| For ongoing maintenance | `doc-maintainer` (after doc-foundation) |
