---
name: prd-to-issues
description: Break a PRD, scoped feature plan, or workshop-ready spec into concrete vertical-slice issue files in issues/. Use when the user wants to turn a PRD into tasks, generate AFK or HITL implementation slices, or quickly produce dependency-ordered local issue files from an approved spec.
---

# PRD to Issues

Turn a PRD into thin, independently workable vertical slices. Prefer a fast, recommended breakdown over a long review cycle.

## Workflow

1. Locate the PRD.

- Default to `issues/prd.md` if it exists and the user did not specify another file.
- If the PRD is not already in context, read it from disk.

2. Rebuild the relevant repo context.

- Explore only the codebase seams needed to judge slice boundaries, blockers, and likely module touch points.
- Use the current repo state to avoid proposing slices that depend on nonexistent infrastructure or miss obvious integration paths.

3. Draft vertical slices.

- Break the work into tracer bullets, not horizontal layers.
- Each slice should cut end-to-end through the required layers for that thin behavior.
- Prefer many small demoable slices over a few broad ones.
- Prefer AFK slices unless genuine human review or design choice is required.

## Vertical Slice Rules

- A slice must be narrow but complete.
- A completed slice must be demoable, testable, or otherwise verifiable on its own.
- Do not create "schema only," "API only," or "UI only" slices unless the user explicitly wants infrastructure work separated out.
- Use blockers only when one slice truly cannot proceed without another.
- Prefer dependency chains that keep momentum high and parallelism possible.

## Speed Rules

- Do not force a long quiz round if the PRD is already clear.
- If the user signals urgency or workshop mode, present a compact recommended breakdown first.
- Ask only for the smallest approval needed to avoid writing bad issues.
- If the user clearly wants to move fast, accept reasonable defaults for granularity, HITL/AFK labeling, and dependency structure.

## Presentation

Present the proposed breakdown as a numbered list. For each slice, show:

- Title
- Type: `AFK` or `HITL`
- Blocked by: `None` or the slice titles it depends on
- User stories covered: reference the relevant PRD story numbers
- Why this slice exists: one sentence on the end-to-end behavior it proves

Keep this review concise. If the user is in workshop mode, prefer one pass of "approve or change" over iterative interrogation.

## File Creation

After approval, write each slice as a markdown file in `issues/`.

- Use the naming pattern `issues/NNN-short-title.md`.
- Start numbering from the next available number in `issues/`.
- Create files in dependency order so blocker references can point at real filenames.
- Use local filenames, never GitHub issue numbers.
- Do not call `gh issue create` or any external service.
- Do not modify the parent PRD.

## Issue Template

Write each issue with this structure:

```md
## Parent PRD

`issues/prd.md`

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior it delivers, not a layer-by-layer implementation checklist. Reference the relevant PRD sections or user stories instead of duplicating the entire spec.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- `issues/NNN-title.md`

Or `None - can start immediately` if there are no blockers.

## User stories addressed

- User story 1
- User story 4
```

## Quality Bar

- Acceptance criteria should describe observable behavior, not implementation tasks.
- Titles should be concrete and implementation-friendly.
- Slices should be small enough that one agent or developer can own them cleanly.
- If a slice feels too broad, split it by end-to-end behavior, not by technical layer.
- If several tiny slices only make sense together, merge them.
