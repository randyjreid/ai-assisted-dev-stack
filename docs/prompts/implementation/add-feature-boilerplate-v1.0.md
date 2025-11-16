---
title: Add Feature – Boilerplate Implementation
purpose: Help Claude Code scaffold a new feature with clear acceptance criteria and verification steps
recommended_tool: Claude Code CLI
context_requirements:
  - Feature brief or issue link with desired behavior
  - Target directories and existing modules to integrate with
  - API contracts or UI specs if applicable
  - Tests or fixtures that must be updated
token_budget_notes: Keep prompts under 110k tokens. Summarize large specs; provide links for optional reading instead of inline dumps.
version_history:
  - v1.0 (2025-02-14): Initial feature template
---

## Template
```
Task: Implement <feature-name>
Branch: <branch-name>
Base commit: <sha>

Feature overview:
<One paragraph describing the user story, acceptance criteria, and motivation.>

Files in scope:
- <path/to/module1>
- <path/to/module2>
- <path/to/tests>

Existing behavior:
<Short summary of current behavior and limitations. Include relevant code snippets or API signatures.>

Requirements:
1. <Requirement 1>
2. <Requirement 2>
3. <Requirement 3>
4. Update documentation at <path/to/doc>
5. Add or update tests at <path/to/tests>

Constraints:
- Follow existing coding style guidelines
- Do not introduce new dependencies without approval
- Keep public APIs backward compatible unless otherwise stated
- If design questions arise, stop and list the open questions instead of guessing

Verification:
- Explain which unit/integration tests should be run
- Provide diffs and a summary of new behavior
```

## When to Use
- Adding a new endpoint, CLI flag, or configuration option within an existing subsystem
- Scaffold work where behavior is well defined but repetitive

## When Not to Use
- Exploratory prototypes that are easier to spike manually
- Architectural rewrites that need a design review first

## Worked Example
```
Task: Implement rate limiting toggle for API keys
Branch: feature/api-key-rate-limits
Base commit: 9f11cab

Feature overview:
Allow administrators to enable or disable rate limiting per API key. The UI already exposes a checkbox, but the backend ignores it. When disabled, the limiter should skip counting requests for that key.

Files in scope:
- src/api/keys.js
- src/services/rateLimiter.js
- tests/api/keys.test.js
- docs/configuration.md

Existing behavior:
Keys have a boolean flag `rateLimitEnabled`, but rateLimiter.js does not read it. All keys are limited globally.

Requirements:
1. Update rateLimiter.enforce() to skip limits when rateLimitEnabled === false
2. Add an audit log entry when toggling the flag
3. Expose the flag in the GET /api/keys endpoint
4. Document the behavior in docs/configuration.md
5. Add tests covering both enabled and disabled cases

Constraints:
- Keep response formats unchanged
- Only edit files listed above

Verification:
- Run npm test -- tests/api/keys.test.js
- Provide diffs plus summary of API response changes
```

## Versioning Notes
- Increase the minor version when requirements or verification instructions change.
- Record changes with date and branch reference.
