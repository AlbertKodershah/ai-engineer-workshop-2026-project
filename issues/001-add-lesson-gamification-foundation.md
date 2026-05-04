## Parent PRD

`issues/prd.md`

## What to build

Implement the first end-to-end gamification slice for students. When a student completes a lesson for the first time, Cadence should record a one-time gamification award, update the student's gamification summary, and surface that summary on the dashboard. This slice is the tracer bullet for the PRD's explicit gamification model and should establish the durable foundation later slices reuse for quiz rewards and course completion.

This slice should cover the lesson-completion award path, idempotent duplicate protection, UTC-day streak advancement for qualifying lesson activity, level derivation from total points, and dashboard visibility of the new summary state.

## Acceptance criteria

- [ ] When an enrolled student marks a lesson complete for the first time, Cadence creates exactly one gamification award event and updates the student's total points, level, current streak, longest streak, and last qualifying UTC day.
- [ ] Repeating lesson completion for the same lesson, retrying the same request, or re-entering the completed lesson does not create duplicate rewards or inflate the student's totals.
- [ ] The student dashboard shows a gamification summary alongside existing course progress, including at least total points, current level, current streak, and longest streak, and users with no qualifying activity see a clean zero-state rather than an error.

## Blocked by

None - can start immediately

## User stories addressed

- User story 1
- User story 2
- User story 5
- User story 6
- User story 8
- User story 10
- User story 11
- User story 12
- User story 16
- User story 18
- User story 19
- User story 20
