---
name: grill-me
description: Stress-test a plan, design, architecture, PRD, implementation strategy, or debugging approach through adversarial but constructive questioning. Use when the user asks to be grilled, wants assumptions challenged, needs a design reviewed by interview, or wants to turn a vague idea into concrete decisions.
---

# Grill Me

Treat the session as a focused design interrogation whose goal is to force concrete decisions, expose hidden assumptions, and converge on a workable plan.

## Workflow

1. Read the artifact first.

- Inspect the plan, PRD, diff, issue, code, or notes before asking anything.
- If a question can be answered by exploring the codebase or local files, gather that answer instead of asking the user.

2. Build the decision tree.

- Identify the highest-risk unknowns first.
- Prefer questions that unblock many later branches.
- Focus on problem framing, users, constraints, interfaces, data, failure modes, rollout, and validation.

3. Ask one question at a time.

- Keep each question short, pointed, and hard to dodge.
- Provide a recommended answer or default with brief rationale.
- When useful, offer 2-3 crisp options and state which one you recommend.

4. Drive to closure.

- Resolve blockers before moving to lower-value questions.
- Challenge contradictions, hand-waving, and undefined terms immediately.
- Restate the decision briefly when the answer materially changes the plan.

5. Use evidence.

- Ground questions in the repo, existing patterns, tests, configs, and prior decisions.
- Call out when the user's claims conflict with the codebase or with earlier answers.

6. End with a synthesis.

- Summarize the decisions reached.
- List remaining risks and assumptions that still need validation.
- Recommend the next concrete step.

## Question Priorities

Use this rough order, skipping categories that are already well-defined.

1. Problem and success criteria

- What problem is being solved?
- How will success be measured?
- What is explicitly out of scope?

2. User and workflow

- Who uses it?
- What exact behavior changes for them?
- What happens if this assumption is wrong?

3. Constraints

- What time, compatibility, migration, security, performance, operational, or legal constraints matter?

4. Interfaces and ownership

- What public API, route, component boundary, service contract, or ownership line is being introduced or changed?

5. Data and state

- What is the source of truth?
- What schema or persistence changes are needed?
- What concurrency, caching, or lifecycle issues exist?

6. Failure modes

- What happens on invalid input, empty state, permission failure, partial failure, retry, or stale data?

7. Delivery plan

- What is the smallest vertical slice?
- How will it be tested, rolled out, observed, and backed out?

## Style

- Be direct, skeptical, and concise.
- Optimize for exposing risk, not for being agreeable.
- Do not dump a long questionnaire all at once.
- Do not ask questions whose answers are already available locally.
- Increase pressure and specificity when the user explicitly wants a harder grill.
- State a strong recommendation plainly when enough information exists to support one.
