## Problem Statement

Cadence has basic lesson progress tracking, but students do not get a meaningful sense of momentum or accomplishment as they work through a course. The current experience shows lesson completion and course progress percentages, but it does not accumulate visible personal progress over time, reward consistent engagement, or create any incentive to complete quizzes. Product feedback indicates that this contributes to students dropping off after a few lessons because their work feels invisible and unrewarded.

Cadence needs a private, professional-feeling gamification layer that reinforces progress without turning the platform into a competitive social product. The feature must help students feel that progress compounds over time, make quizzes feel worthwhile, and encourage daily return behavior through streaks.

## Solution

Add a private student gamification system for `v1` built around points, levels, and streaks. Students earn points for one-time accomplishments only: first-time lesson completion, first quiz pass, and course completion. These accomplishments feed an explicit gamification data model rather than being derived ad hoc from existing progress tables.

The dashboard becomes the primary place where students see their gamification status: total points, current level, current streak, longest streak, and recent progress context. Streaks are based on UTC calendar days and count any point-awarding accomplishment as an active day. The system starts tracking from launch forward only; it does not retroactively backfill historical student activity.

The implementation should stay workshop-friendly and `v1`-sized: no leaderboards, no competitive features, no timezone customization, no historical migration, and no rewards for repeat or farmable behavior such as quiz retries.

## User Stories

1. As a student, I want to see my total points, so that my progress feels cumulative rather than invisible.
2. As a student, I want to earn points when I complete a lesson for the first time, so that regular course progress feels rewarding.
3. As a student, I want to earn points when I pass a quiz for the first time, so that quizzes feel worthwhile.
4. As a student, I want to earn an additional reward when I finish a course, so that completing a full learning path feels meaningful.
5. As a student, I want my level to update automatically from my total points, so that I can see long-term progress at a glance.
6. As a student, I want to see my current streak, so that I know whether I am maintaining consistent daily learning.
7. As a student, I want to see my longest streak, so that I have a personal milestone to aim to beat.
8. As a student, I want any meaningful learning accomplishment on a day to preserve my streak, so that I do not have to guess which activity "counts."
9. As a student, I want quiz retries to improve my learning without generating repeat rewards, so that the system feels fair and cannot be gamed easily.
10. As a student, I want repeat lesson submissions to avoid awarding duplicate points, so that accidental double-submits do not corrupt my progress.
11. As a student, I want my gamification progress to be private to me, so that I get motivation without public competition.
12. As a student, I want my dashboard to surface both learning progress and gamification progress together, so that I can understand where I am and what to do next.
13. As a student, I want my streak to continue only across qualifying days, so that the metric reflects real consistency rather than raw time elapsed.
14. As a student, I want my streak to reset after missing qualifying days, so that the metric remains credible.
15. As a student, I want my existing pre-launch progress not to be silently reinterpreted into rewards, so that the new system starts from a clear and predictable point in time.
16. As a student, I want the system to handle partial failures safely, so that a lesson completion or quiz pass does not award points twice if the request is retried.
17. As an instructor or admin, I want the student-facing gamification feature to avoid changing course authoring workflows, so that the initial release stays low-risk.
18. As the product team, I want an explicit record of why points were awarded, so that future tuning, debugging, and support are possible.
19. As the product team, I want a minimal `v1` that can be demonstrated end-to-end quickly, so that the feature can be delivered within the workshop and iterated later.
20. As the engineering team, I want implementation decisions and scope boundaries captured clearly, so that the feature can be broken into clean vertical slices afterward.

## Implementation Decisions

- Introduce an explicit gamification layer as the source of truth instead of inferring points, levels, or streaks directly from `lesson_progress` and `quiz_attempts`.
- Add a gamification event ledger that records each qualifying award event once. At minimum, the ledger should support event type, user, related entity, awarded points, and award timestamp.
- Add a per-user gamification summary model that stores current totals and cached streak state needed for fast dashboard reads. At minimum, this should support total points, current streak, longest streak, last qualifying activity day, and any level/rank fields that simplify rendering.
- Award points only for one-time accomplishments in `v1`. Qualifying accomplishments are:
  - first-time lesson completion,
  - first quiz pass for a quiz,
  - course completion.
- Do not award points for quiz failures, quiz retakes after the first pass, lesson re-completions, video watch events, or generic "activity."
- Count streaks by UTC calendar day. Any point-awarding accomplishment on a UTC day counts as a qualifying day for streak purposes.
- Do not award extra streak bonus points in `v1`. Streaks are motivational state, not an additional points multiplier yet.
- Do not backfill historical lesson completions, quiz passes, or completed courses. Gamification starts from launch forward.
- Keep the feature private to the student. No leaderboard, peer comparison, team comparison, or instructor-facing ranking UI is part of `v1`.
- Derive levels from total points using fixed thresholds defined in application code for `v1`. Threshold values and rank labels should be treated as tuning constants rather than a full admin-managed system.
- Update the lesson completion flow so that a first-time lesson completion can trigger a gamification award safely and idempotently.
- Update the quiz submission flow so that the first passing attempt can trigger a gamification award safely and idempotently, while later passing retries do not create duplicate rewards.
- Explicitly handle course completion in runtime logic. The current domain model already has enrollment completion state, so `v1` should ensure course completion is marked when the last required lesson is completed and should award the course-completion reward only once.
- Centralize award logic in a dedicated gamification service or equivalent boundary instead of scattering award rules across route modules. This service should own qualification checks, duplicate protection, streak updates, point totals, and level derivation.
- Keep award logic resilient to request retries and repeated user actions. Duplicate event prevention must be enforced by both service logic and persistence constraints where practical.
- Extend the dashboard loader and UI to include gamification summary data alongside existing course progress data.
- Surface gamification prominently on the student dashboard. The dashboard is the required `v1` display surface; adding gamification UI to other screens is optional and secondary.
- Keep the initial release narrowly scoped around lessons, quizzes, course completion, and dashboard visibility. Badge systems, achievements, rewards catalogs, and notifications are deferred.

## Testing Decisions

- Good tests should verify external behavior through public interfaces and real state transitions, not implementation details or private helper structure.
- The highest-risk area is idempotent awarding behavior. Tests should prove that repeated lesson completion requests and repeated quiz submissions do not award duplicate points.
- The dedicated gamification service should be tested heavily at the boundary level because it will own award qualification, event creation, total point updates, and streak updates.
- The lesson completion integration path should be tested to confirm that first-time lesson completion both updates lesson progress and triggers the correct gamification result.
- The quiz submission integration path should be tested to confirm that:
  - failed attempts award nothing,
  - the first passing attempt awards points,
  - later passing attempts do not create duplicate rewards.
- Course completion behavior should be tested to confirm that finishing the final lesson marks the enrollment complete and awards the course-completion reward exactly once.
- Streak logic should be tested around UTC day boundaries, same-day multiple accomplishments, consecutive qualifying days, missed days, and longest-streak updates.
- Dashboard loader or route-level tests should verify that gamification summary data is exposed correctly to the student-facing UI.
- Prior art exists in the repo's service-heavy test suite, especially the progress, enrollment, purchase, coupon, and quiz-related service tests. New tests should follow the same style of validating behavior against the real SQLite-backed service layer where possible.
- UI tests should stay focused on visible outcome and rendering of summary state rather than implementation-level component details.

## Out of Scope

- Leaderboards, public rankings, team rankings, or any competitive social mechanics.
- Retroactive backfill of historical lesson, quiz, or course activity.
- Per-user timezone support for streaks.
- Streak bonus points, multipliers, freeze mechanics, or streak repair flows.
- Badges, achievements, trophies, collectible rewards, or a reward redemption system.
- Instructor-facing or admin-facing gamification analytics dashboards.
- Email, push, or in-app notification campaigns tied to streaks or level-ups.
- A configurable admin UI for point values, thresholds, or rank names.
- Rewards for passive activity such as video watch events, page views, or time spent.
- Purchase, team, coupon, or PPP-specific gamification behavior.
- Reworking course authoring or quiz authoring flows.

## Further Notes

- The current codebase already tracks lesson progress, quiz attempts, and enrollment completion state, but it does not yet have a gamification model or a guaranteed runtime course-completion path tied to lesson completion. The PRD assumes that gap will be closed as part of this feature.
- Because `v1` starts without backfill, rollout communication should make it clear that gamification begins from launch onward.
- UTC-based streak logic is intentionally a simplification to avoid blocking on profile timezone support during the workshop. If the feature proves valuable, per-user timezone handling is a likely future extension.
- Point values and level thresholds should be chosen early in implementation, but they are tuning constants rather than architecture blockers. They can live in code for `v1`.
- This PRD is intentionally shaped to support a clean follow-up issue breakdown into vertical slices such as schema plus ledger foundation, live award hooks, streak plus summary calculation, and dashboard presentation.
