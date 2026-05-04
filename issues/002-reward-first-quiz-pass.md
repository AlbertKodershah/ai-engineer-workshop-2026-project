## Parent PRD

`issues/prd.md`

## What to build

Extend the gamification foundation so quiz submission participates in the same reward system. A student's first passing attempt for a quiz should award points and update the student's summary, while failed attempts and later passing retries should not create repeat rewards. This slice proves that the gamification model handles retry-heavy behavior without becoming farmable.

This slice should reuse the event and summary foundation from the lesson path, integrate with the existing quiz submission flow, and preserve the PRD rule that only one-time accomplishments qualify for points.

## Acceptance criteria

- [ ] Failed quiz attempts do not award points, do not create gamification events, and do not change the student's streak state.
- [ ] The first passing attempt for a quiz creates exactly one gamification reward event, updates the student's total points, level, and streak summary, and the updated totals are visible on the dashboard.
- [ ] Additional passing attempts for the same quiz do not create duplicate rewards even if the user retries the quiz repeatedly.

## Blocked by

- `issues/001-add-lesson-gamification-foundation.md`

## User stories addressed

- User story 3
- User story 8
- User story 9
- User story 12
- User story 16
- User story 18
- User story 19
