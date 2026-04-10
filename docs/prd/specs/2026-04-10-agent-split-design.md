# Agent Split Design — Communication Agent + Analysis Agent

## Overview

Split the monolithic PRD Architect (`docs/prd/agent-prompt.md`) into two independent agents:

1. **Communication Agent** — cheap/fast model, interactive, stakeholder-facing. Reads PRDs but never writes files.
2. **Analysis Agent** — capable model, one-shot, manual trigger. Processes conversation transcripts and updates the full PRD set.

A new `docs/prd/open-questions.yml` file provides a machine-readable registry of open questions for downstream consumption (e.g., a UI).

The Iteration Planner remains unchanged as a separate third agent.

---

## Agent 1: Communication Agent

### Purpose

Be the stakeholder-facing conversation layer. A smart PM colleague that asks good questions, challenges vagueness, and keeps things MVP-scoped. Runs on a cheap/fast model to keep costs low for interactive use.

### File: `prompts/comms-agent-prompt.md`

### Reads

- All files in `docs/prd/` (full PRD state)
- `docs/prd/open-questions.yml` (current unresolved questions)

### Writes

Nothing. Zero file operations.

### Output

The conversation transcript (the chat history itself). This is the input artifact for the Analysis Agent.

### Session Lifecycle

**Bootstrap:**

1. Read all files in `docs/prd/` to load current product state.
2. Read `docs/prd/open-questions.yml` to know what's unresolved.
3. If product is new (empty/missing overview): open with "Tell me about the product you're building."
4. If PRDs exist: give a brief status. If open questions exist, surface them upfront: "Last time we drafted the banner wizard — there are 4 open questions. Want to tackle those, or work on something new?"

**During conversation:**

- Same behavioral rules as the current PRD Architect: natural dialog, minimal questions (1 for short input, 2-3 for brain dumps), non-technical by default, MVP guardian, UX advocate, honest about gaps, no invention.
- When the stakeholder answers an open question, acknowledge conversationally ("Got it — so the rule is one active subscription at a time") but do not update any files. The answer lives in the transcript.
- When discussing a new feature, conduct the intake conversation but do not draft a feature doc. Talk through the feature until the stakeholder is satisfied or steps away.
- No Phase 1/Phase 2 file-writing split. The entire conversation is intake/discussion.

**Session end:**

- Wrap up conversationally. No commit, no changelog write, no file operations.
- The transcript is the deliverable.

### Behavioral Rules (carried from current prompt)

1. **Natural Dialog** — conversational, direct, human. No interview-style questioning.
2. **Minimal Questions** — 1 question for short input, 2-3 for brain dumps. Never more than 3 before moving on.
3. **Non-Technical by Default** — users, actions, outcomes. No tech details unless the stakeholder raises them.
4. **MVP Guardian** — KISS and YAGNI. Challenge scope creep.
5. **UX Advocate** — push for premium UX. Challenge clunky flows.
6. **Honest About Gaps** — name the specific problem, propose a concrete alternative. Never just "can you clarify?"
7. **No Invention** — never fabricate requirements or assume business decisions.

### What the Communication Agent Does NOT Do

- Write or update any PRD files
- Create git commits or tags
- Run gap detection
- Announce file operations (there are none)
- Say "I've drafted X" — instead: "I think we've covered enough to draft X — 7 user stories and a few business rules came out of this."

---

## Agent 2: Analysis Agent

### Purpose

Process a stakeholder conversation transcript and update the PRD set accordingly. One-shot, no dialogue, manual trigger. Runs on a capable model for deep analytical work.

### File: `prompts/analysis-agent-prompt.md`

### Reads

- Conversation transcript (provided as input)
- All files in `docs/prd/` (current PRD state)
- `docs/prd/open-questions.yml` (current open questions)

### Writes

- Feature docs (`docs/prd/features/<slug>.md`)
- `docs/prd/overview.md` (feature index updates)
- `docs/prd/decisions.md` (change log for requirement modifications)
- `docs/prd/changelog.md` (session summary)
- `docs/prd/open-questions.yml` (add/remove questions)
- Git commits and tags

### Processing Pipeline

Execute in this order:

1. **Extract facts from transcript** — identify new features discussed, decisions made, answers to open questions, changes to existing requirements, and new ambiguities surfaced.

2. **Resolve open questions** — if the stakeholder answered an open question during the conversation, remove the `[OPEN]` marker from the feature doc and remove the entry from the YAML file.

3. **Create or update feature docs** — write new feature docs for new features, update existing ones for modifications. Follow the exact document format from CLAUDE.md (frontmatter, summary, user stories, business rules, open questions). Apply versioning rules:
   - Minor bump (`1.0` -> `1.1`) for additive or clarifying changes
   - Major bump (`1.x` -> `2.0`) for changes that alter existing user stories, remove functionality, or change business rules

4. **Gap detection** — scan affected feature docs for:
   - User stories that are vague or missing the "so that" benefit
   - Business rules that contradict each other or other feature docs
   - Missing business rules for scenarios the user stories imply
   - All gaps become `[OPEN]` markers in the feature doc AND entries in the YAML file.

5. **Update overview.md** — add new features to the Feature Index if any were created.

6. **Log decisions** — append entries to `decisions.md` for any requirement changes (not for new features — only modifications to existing requirements).

7. **Write changelog** — append a session entry to `changelog.md` with topics discussed, files created/modified, and open action items.

8. **Commit** — commit all changes. Create git tags for major version bumps (`prd/<feature-slug>/vX.0`).

### What the Analysis Agent Does NOT Do

- Ask questions or conduct dialogue
- Make business decisions — ambiguity becomes `[OPEN]`, never a guess
- Write implementation details
- Invent requirements not present in the transcript

---

## Open Questions YAML File

### Location

`docs/prd/open-questions.yml`

### Structure

```yaml
questions:
  - id: BANNER-OQ-001
    feature: banner-generator
    summary: "Style preset list — what style dimensions beyond dark/light?"
    context: >
      Dark/light is confirmed. Need to define the full set of visual presets
      for the style step (minimal vs. bold, monochrome vs. colorful, etc.)
    related_rules:
      - BANNER-BR-006
    suggested_answers:
      - "Minimal / Bold"
      - "Monochrome / Colorful"
      - "Flat / Photorealistic"
    created: 2026-04-10

  - id: BANNER-OQ-002
    feature: banner-generator
    summary: "Multiple banners per product or single hero banner for MVP?"
    context: >
      Marketplace listings allow multiple images. Does the stakeholder need
      to generate a set per product, or is one hero banner enough?
    related_rules: []
    suggested_answers:
      - "Single hero banner for MVP"
      - "Set of 3-5 banners per product"
    created: 2026-04-10
```

### Field Definitions

| Field | Required | Description |
|-------|----------|-------------|
| `id` | yes | `<FEATURE_PREFIX>-OQ-<NNN>` pattern |
| `feature` | yes | Feature slug, maps to filename in `features/` |
| `summary` | yes | One-liner shown to the stakeholder |
| `context` | yes | Background for understanding the question without reading the PRD |
| `related_rules` | yes | Business rule IDs this question affects (can be empty list) |
| `suggested_answers` | no | Possible answers proposed by the Analysis Agent |
| `created` | yes | Date the question was first identified |

### Lifecycle

- Analysis Agent **adds** entries when gap detection finds new ambiguities
- Analysis Agent **removes** entries when the transcript shows the stakeholder answered the question
- `[OPEN]` markers in PRD files and YAML entries are kept in sync — both added together, both removed together

### ID Scheme Addition

| Type | Pattern | Example |
|------|---------|---------|
| User Story | `<FEAT>-US-<NNN>` | `BANNER-US-001` |
| Business Rule | `<FEAT>-BR-<NNN>` | `BANNER-BR-001` |
| Open Question | `<FEAT>-OQ-<NNN>` | `BANNER-OQ-001` |

---

## File Layout

### Prompt files

```
prompts/
├── comms-agent-prompt.md        # Communication Agent
├── analysis-agent-prompt.md     # Analysis Agent (PRD writer)
├── iteration-planner-prompt.md  # Iteration Planner (unchanged)
```

The old `docs/prd/agent-prompt.md` is deleted — fully replaced by the two new prompts.

### PRD directory addition

```
docs/prd/
├── overview.md
├── iterations.md
├── decisions.md
├── changelog.md
├── open-questions.yml           # NEW — managed by Analysis Agent
├── features/
│   └── ...
```

---

## CLAUDE.md Updates

### Agent Configuration section

Replace the current PRD Architect reference with:

> **Communication Agent** prompt lives at `prompts/comms-agent-prompt.md`. Load it as the system prompt when starting a stakeholder session.
>
> **Analysis Agent** prompt lives at `prompts/analysis-agent-prompt.md`. Run it one-shot after a stakeholder session, providing the conversation transcript as input.
>
> **Iteration Planner** prompt lives at `prompts/iteration-planner-prompt.md`. Run it one-shot to regenerate `docs/prd/iterations.md` from current PRDs.

### Directory Layout section

Add `open-questions.yml` to the directory tree.

### Requirement ID Scheme table

Add the Open Question row (`<FEAT>-OQ-<NNN>`).

---

## What Does NOT Change

- Document formats (feature doc structure, decisions.md, changelog.md) — defined in CLAUDE.md, both agents follow them
- Versioning rules (minor/major bumps, git tags on major bumps)
- The Iteration Planner — remains a separate third agent, unchanged
- The overall PRD philosophy — MVP-focused, stakeholder-driven, no invented requirements
