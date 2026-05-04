## Parent PRD

`issues/prd.md`

## What to build

Complete the `v1` gamification loop by wiring course completion and full streak behavior into the live product flow. When a student completes the final required lesson in a course, Cadence should mark the enrollment complete, award a one-time course-completion reward, and keep current-streak and longest-streak state correct across qualifying and missed UTC days. The dashboard should reflect both the completed-course state and the updated gamification summary.

This slice closes the remaining product gap called out in the PRD: enrollment completion exists in the model today, but it is not clearly tied to the live lesson-completion path or to a course-completion reward.

## Acceptance criteria

- [ ] Completing the final incomplete lesson in a course marks the student's enrollment complete and creates exactly one course-completion gamification reward event.
- [ ] Reopening completed lessons, repeating completion requests, or revisiting an already completed course does not create duplicate course-completion rewards or corrupt completion state.
- [ ] Streak logic correctly advances across consecutive qualifying UTC days, resets after missed days, preserves longest streak, and the dashboard reflects the resulting streak state together with the completed-course view.

## Blocked by

- `issues/001-add-lesson-gamification-foundation.md`

## User stories addressed

- User story 4
- User story 7
- User story 12
- User story 13
- User story 14
- User story 15
- User story 19
- User story 20
