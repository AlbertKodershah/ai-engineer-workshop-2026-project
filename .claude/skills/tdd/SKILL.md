---
name: tdd
description: Behavior-first test-driven development with a vertical red-green-refactor loop. Use when the user wants TDD, mentions red-green-refactor, wants a feature or bugfix built test-first, or needs help choosing high-value behavioral tests in this codebase.
---

# Test-Driven Development

Default to pragmatic, behavior-first TDD. Keep the loop tight, test through real boundaries, and avoid spending the whole task in planning mode.

See [tests.md](tests.md) for examples, [mocking.md](mocking.md) for boundary mocking guidance, [interface-design.md](interface-design.md) for testable interface rules, and [refactoring.md](refactoring.md) for post-green cleanup ideas.

## Core Rules

- Test observable behavior through a public boundary, not private helpers.
- Write one failing test at a time.
- Implement only enough code to make the current test pass.
- Prefer vertical slices over horizontal batches of tests or code.
- Refactor only after returning to green.

## Anti-Patterns

- Do not write all tests first and all implementation later.
- Do not test internal call structure, private methods, or collaborator invocation counts unless the collaborator is a true system boundary.
- Do not over-mock your own modules just to make tests easier.
- Do not ask the user to pre-approve every test if the issue or PRD already makes the target behavior clear.

## Workflow

1. Choose the boundary and first behavior.

- Infer the public boundary from the issue, PRD, or existing code whenever it is clear.
- Ask the user only if there are multiple materially different interface choices or test scopes with real product impact.
- Pick the smallest end-to-end behavior that proves the slice works.
- Prefer the highest-risk behavior first: idempotence, permissions, state transitions, or the main happy path.

2. Write the first failing test.

- Express the test in terms of behavior a caller or user would care about.
- Name the test after the capability, not the implementation.
- Run the narrowest test command that exercises just that behavior.

3. Make it pass with minimal code.

- Change only the code needed for the current behavior.
- Avoid building future functionality early.
- Re-run the same narrow test until green.

4. Repeat one behavior at a time.

- Add the next failing test only after the current one is green.
- Let each new test respond to what the previous cycle revealed.
- Keep expanding the behavior surface in thin slices.

5. Refactor after green.

- Extract duplication.
- Deepen shallow modules.
- Move complex logic behind small, stable interfaces.
- Re-run tests after each refactor step.

6. Finish with broader verification.

- Run the relevant targeted test file(s).
- Run `npm test` before closing the task.
- Run `npm run typecheck` before closing the task.

## Repo Guidance

In this repo, prefer service-boundary tests for business logic.

- Many existing tests follow the pattern in `app/services/*.test.ts`.
- Use the in-memory SQLite test database from `app/test/setup.ts` when testing data-heavy services.
- Mock `~/db` at the module boundary so the service under test uses the fresh test database.
- Seed the smallest base data needed, then add only the records required for the behavior under test.

When the route layer is thin:

- Push complex logic into a service or deep module.
- Test the service heavily.
- Keep route tests for integration behavior, request branching, and returned data shape only when that behavior is the real risk.

When external boundaries are involved:

- Mock only the true boundary: time, randomness, external APIs, filesystem, or similar.
- Do not mock your own service layer if you can test the real behavior through the public interface.

## Fast Defaults

- If the user asked for TDD and did not specify exact tests, choose the highest-value behaviors yourself.
- In workshop mode, prefer a tracer bullet plus 2-3 follow-up behaviors over exhaustive edge-case coverage.
- If the codebase already shows an established test style, follow it instead of inventing a new one.

## Per-Cycle Checklist

- The test describes behavior, not implementation.
- The test uses a public interface or real boundary.
- The test would survive an internal refactor.
- The implementation is minimal for the current test.
- No speculative behavior was added.
