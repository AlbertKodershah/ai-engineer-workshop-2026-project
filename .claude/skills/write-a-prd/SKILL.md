---
name: write-a-prd
description: Turn a client brief, feature request, workshop idea, or partially-decided plan into a pragmatic PRD written to issues/prd.md. Use when the user wants a PRD, wants to scope a feature, wants to convert a brief or Slack request into product requirements, or needs a fast workshop-friendly spec with explicit assumptions and implementation/testing decisions.
---

# Write a PRD

Default to a pragmatic, decision-oriented PRD. Prefer drafting from the brief and repo context instead of forcing a long discovery interview.

## Workflow

1. Read the artifact and the repo.

- Read the client brief, issue, notes, or request first.
- Explore the repo to verify the current state, constraints, and relevant seams.
- Summarize what is already known before asking the user anything.

2. Resolve only blocking decisions.

- Ask only the smallest set of questions needed to avoid a bad PRD.
- Prefer recommended defaults over open-ended brainstorming.
- Ask one high-leverage question at a time when clarification is needed.
- If the user signals time pressure or workshop mode, compress aggressively:
  - summarize the remaining forks,
  - recommend defaults,
  - lock accepted defaults quickly,
  - move on.

3. Shape the implementation.

- Identify the main modules or boundaries likely to change.
- Prefer small vertical slices and deep modules over broad horizontal work.
- Capture schema changes, route or API contracts, state ownership, failure modes, and rollout assumptions.
- Decide what deserves tests based on behavioral risk, not completeness theater.

4. Write the PRD to `issues/prd.md`.

- Create `issues/` if it does not exist.
- Write decisive prose and record assumptions explicitly instead of stalling on every unknown.
- Keep the document product-facing, but make implementation and testing decisions concrete enough to guide issue breakdown.
- Do not create a GitHub issue or call external services.

5. Sanity-check the scope.

- Ensure the proposed `v1` is coherent, demoable, and small enough for the user's timeline.
- Push non-essential work into `Out of Scope`.
- If unresolved blockers remain, record them in `Further Notes`.

## Speed Rules

- Do not require the user to provide a long description if a brief already exists.
- Do not grill the user on every branch when a reasonable default exists.
- Do not turn tuning questions into blockers unless they change architecture or scope.
- Prefer the smallest viable `v1` when the brief is ambiguous.
- Prefer explicit assumptions over fake certainty.

## PRD Content Rules

### Problem Statement

- Describe the problem from the user's perspective.
- State the pain clearly and concretely.

### Solution

- Describe the proposed solution from the user's perspective.
- Keep it focused on the intended `v1`.

### User Stories

- Write a numbered list in the form `As an <actor>, I want a <feature>, so that <benefit>`.
- Cover the main actors, primary flows, permissions, edge cases, empty states, and failure paths.
- Be thorough, but do not pad the list with trivial restatements.

### Implementation Decisions

- Record the main modules or boundaries that will change.
- Record interface, schema, API, state, rollout, and migration decisions.
- Record important assumptions and defaults that were chosen to keep momentum.
- Do not include file paths or code snippets.

### Testing Decisions

- State that good tests verify external behavior through public interfaces, not implementation details.
- Record which modules or boundaries should be tested.
- Mention relevant prior art in the repo when it exists.
- Focus on high-risk behaviors, idempotence, permissions, and regressions.

### Out of Scope

- Record non-`v1` items, deferred tuning, nice-to-haves, and adjacent ideas.

### Further Notes

- Record follow-up risks, open questions, backfill assumptions, rollout notes, or future extensions.

## Template

Write the PRD using this structure:

```md
## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

1. As an <actor>, I want a <feature>, so that <benefit>

## Implementation Decisions

- Decision 1

## Testing Decisions

- Testing decision 1

## Out of Scope

- Out of scope item 1

## Further Notes

- Note 1
```
