# PRD Repository

This repository maintains Product Requirements Documents for the project. An AI agent (the PRD Architect) maintains these docs through stakeholder conversations.

## Agent Configuration

The PRD Architect system prompt lives at `docs/prd/agent-prompt.md`. Load it as the system prompt when starting a stakeholder session.

## Directory Layout

```
docs/prd/
├── overview.md              # Product vision, goals, scope, target users
├── decisions.md             # Append-only decision log (what changed, when, why)
├── changelog.md             # Per-session conversation summaries
├── features/
│   ├── <feature-slug>.md    # One file per feature/epic
│   └── ...
```

## Document Formats

### overview.md

Top-level product document. Contains:

- **Product Vision** — what the product is and why it exists
- **Goals** — measurable objectives
- **Target Users** — who uses this and in what context
- **Scope** — what's in and what's explicitly out
- **Cross-Cutting Concerns** — auth model, data privacy, performance targets, accessibility
- **Feature Index** — links to all feature docs with their current status

### Feature Docs (features/<slug>.md)

Each feature gets its own file. The slug should be lowercase, hyphen-separated, and descriptive (e.g., `user-auth`, `billing`, `notification-preferences`).

**Frontmatter (required):**

```yaml
---
title: <Feature Name>
version: "1.0"
status: draft | active | deprecated
last_updated: YYYY-MM-DD
stakeholder: <name or role>
---
```

**Sections (required, in this order):**

1. **Summary** — 2-3 sentence description of the feature and its purpose.

2. **User Stories** — Each story uses the format: `As a <role>, I want <goal>, so that <benefit>`. Each story gets a unique ID using the pattern `<FEATURE>-US-<NNN>` (e.g., `AUTH-US-001`).

3. **Business Rules** — Concrete constraints and logic governing the feature. Each rule gets a unique ID using the pattern `<FEATURE>-BR-<NNN>` (e.g., `AUTH-BR-001`). Each rule references the user story ID(s) it serves.

4. **Open Questions** — Unresolved items marked with `[OPEN]` and enough context to understand what needs deciding. Remove entries as they are resolved.

### decisions.md

Append-only log. Each entry uses this format:

```markdown
## YYYY-MM-DD — <Short title>

- **Feature:** <feature slug>
- **Requirement IDs affected:** <comma-separated list>
- **Change:** <what changed>
- **Rationale:** <why it changed>
- **Requested by:** <stakeholder name or role>
- **Version bump:** <feature-slug version X.Y → X.Z>
```

### changelog.md

Per-session log. Each entry uses this format:

```markdown
## YYYY-MM-DD — Session with <stakeholder>

- **Topics discussed:** <comma-separated list>
- **PRD files created:** <list of file paths, or "none">
- **PRD files modified:** <list of file paths, or "none">
- **Open action items:** <list, or "none">
```

## Versioning

- Feature docs carry a `version` field in frontmatter using semver-style numbering: `1.0`, `1.1`, `2.0`
- **Minor bump** (`1.0` → `1.1`): additive or clarifying changes that don't alter existing user stories or business rules
- **Major bump** (`1.x` → `2.0`): breaking changes — altered user stories, removed functionality, changed business rules
- Commits happen at natural conversation boundaries: after a feature draft is created, after modifications are complete, and at session end
- Git tags are created on major version bumps using the pattern: `prd/<feature-slug>/v<major>.0`

## Requirement ID Scheme

All IDs use the pattern `<FEATURE_PREFIX>-<TYPE>-<NNN>`:

| Type | Pattern | Example |
|------|---------|---------|
| User Story | `<FEAT>-US-<NNN>` | `AUTH-US-001` |
| Business Rule | `<FEAT>-BR-<NNN>` | `AUTH-BR-001` |

The feature prefix is an uppercase abbreviation of the feature slug (e.g., `user-auth` → `AUTH`, `billing` → `BILL`).
