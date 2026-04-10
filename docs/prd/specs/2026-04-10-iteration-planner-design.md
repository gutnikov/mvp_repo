# Iteration Planner Agent — Design Spec

## Overview

A one-shot agent that reads all PRDs in `docs/prd/` and produces `docs/prd/iterations.md` — an ordered list of vertical slices ready for development. Each iteration is a user-touchable deliverable: something you can put in front of the stakeholder and say "go try this."

Separate from the PRD Architect. The PRD Architect talks to stakeholders and writes requirements. The Iteration Planner reads those requirements and plans delivery.

## Core Principle

**Vertical sliced delivery.** Every iteration delivers a user-facing update that can be tested by the stakeholder. Iterations are isolated — never mix features. The litmus test: "Can I send the stakeholder a message saying: we built X, test it like this: Y?"

## Agent Configuration

- **Prompt location:** `docs/prd/iteration-planner-prompt.md`
- **Mode:** One-shot. No conversation, no questions. Read docs, produce plan, commit, done.
- **Output:** `docs/prd/iterations.md` (regenerated from scratch on every run)
- **PRD access:** Read-only for all files in `docs/prd/` except `iterations.md` (write)

## Trigger

Run the planner whenever PRDs change: new feature drafted, open questions resolved, requirements modified. Each run regenerates the full iterations file from the current PRD state.

## Bootstrap

On load, the agent silently reads:

1. `docs/prd/overview.md` — to discover all features
2. Every file in `docs/prd/features/` — to get stories, business rules, and open questions
3. `docs/prd/iterations.md` (if it exists) — not used for planning; overwritten

## Slicing Logic

### Step 1 — Classify stories as "ready" or "blocked"

- A story is **blocked** if any business rule that serves it (identified by the `(serves BANNER-US-XXX)` annotation on each rule) has an `[OPEN]` marker, or if the story is referenced by an open question in the feature doc's Open Questions section.
- Everything else is **ready**.

### Step 2 — Group ready stories into touchable slices

Apply the touchable-slice test: *"Can I write a message to the stakeholder that says: we built X, go test it by doing Y?"*

Grouping heuristics:

- Stories sharing the same wizard step or UI surface cluster together.
- A story that produces output (e.g., "download the banner") anchors an iteration — pull in everything upstream needed to reach that output.
- An input-only story (e.g., "enter product name") never stands alone — it joins the iteration where its input first becomes visible in a result.

### Step 3 — Order iterations by dependency

- If iteration B needs the output of iteration A, A comes first.
- Among independent iterations, prefer the one delivering the most tangible user value first.

### Step 4 — Park blocked stories

Blocked stories go into a "Blocked" section at the bottom. Grouped by which open question blocks them. No iteration number, no ordering — they're parked until the stakeholder resolves the question.

## Output Format

```markdown
# Iteration Plan

Generated from PRDs on YYYY-MM-DD. Re-run the iteration planner when PRDs change.

## Iteration 1 — <Short descriptive name>

### What we're building
<2-3 sentences: what the user gets at the end of this iteration>

### User stories
- BANNER-US-001: <story title>
- BANNER-US-003: <story title>

### Business rules
- BANNER-BR-001, BANNER-BR-004, BANNER-BR-005

### Acceptance criteria
<Bulleted list — concrete, testable conditions that prove the iteration works>

### Stakeholder message
> Hey, we built <thing>. You can test it like this: <steps>. What do you think?

---

## Iteration 2 — ...

(same structure)

---

## Blocked

Stories that can't be planned yet because of unresolved open questions.

| Story | Blocked by | Open question |
|-------|-----------|---------------|
| BANNER-US-003 | BANNER-BR-006 | Style preset list not yet defined |

Nothing to do here — stakeholder is working on clarifying these.
```

### Section descriptions

- **What we're building** — dev-facing summary, enough context to start implementation.
- **User stories / Business rules** — traceability back to the PRD. IDs only for business rules (details live in the feature doc).
- **Acceptance criteria** — definition of done. The iteration ships when these all pass.
- **Stakeholder message** — copy-pasteable blurb in plain language. Tells the stakeholder exactly what was built and how to test it.
- **Blocked** — honest list of what's waiting and why, without creating fake iterations.

## Edge Cases

| Scenario | Behavior |
|----------|----------|
| All stories in a feature are blocked | No iterations for that feature. Output: "No iterations can be planned — all stories depend on unresolved open questions." |
| No feature docs exist | Output: "No feature docs found. Run a session with the PRD Architect first." |
| Multiple features exist | Plan across all features. Each iteration stays within one feature unless features are genuinely coupled. |
| Re-run after PRD changes | Full regeneration. `iterations.md` is a derived artifact, not an append-only log. |

## Commit Behavior

After writing `iterations.md`, commit with message: `docs: regenerate iteration plan`. No tags, no version bumps — this is a projection of PRD state, not a source-of-truth document.

## What the Agent Does NOT Do

- **No implementation details** — no tech choices, architecture, or file structure
- **No time estimates** — just ordered slices
- **No prioritization beyond dependency ordering** — the developer decides what to pick up
- **No PRD modification** — read-only access to all PRD files
- **No conversation** — one-shot execution, no questions asked
