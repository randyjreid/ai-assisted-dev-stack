---
title: Test Failure Analysis
purpose: Use Claude Code or Codex to summarize failing tests and propose fixes
recommended_tool: Claude Code CLI (analysis) or OpenAI Codex CLI for second opinion
context_requirements:
  - Failing test names and stack traces
  - Recent commits that may have introduced the regression
  - Files under suspicion
  - Commands used to reproduce the failure
token_budget_notes: Keep prompts under 70k tokens. Include only relevant parts of logs or stack traces.
related_prompts:
  - fix-bug-analysis-v1.0.md
  - partial-failure-followup-v1.0.md
  - code-review-claude-v1.0.md
  - diagnose-drift-or-hallucination-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial template for analyzing failing tests
---

## Template
```
Task: Diagnose failing test(s)
Branch: <branch-name>
Base commit: <sha>

Failing tests:
- <command> → <error summary>

Logs / stack traces:
<snippets>

Suspect files:
- <file path>

Expectations:
1. Summarize probable root cause(s)
2. Recommend code areas to inspect or modify
3. Propose tests to add or update
4. Flag any gaps in the current data

Constraints:
- Do not modify files yet
- Emphasize clarity over speculation

Output format:
- Root cause hypotheses
- Suggested fixes/tests
- Outstanding questions
```

## When to Use
- After CI failure when logs are noisy
- During triage to determine ownership

## When Not to Use
- Known flaky tests already tracked elsewhere
- Failures with obvious fixes that do not need tooling support

## Worked Example
```
Task: Diagnose failing test `Invoices › creates invoice with discount`
Branch: feature/discounts-refactor
Base commit: 55529ac

Failing tests:
- npm test -- tests/invoices.test.js → FAIL (TypeError: amount.toFixed is not a function)

Logs:
- tests/invoices.test.js line 88 stack trace attached

Suspect files:
- src/services/invoices.js
- src/utils/discounts.js

Expectations:
1. Summarize probable root cause
2. Recommend fixes/tests
3. Call out missing info
```

## Versioning Notes
- Update version when output format or expectations change.
