---
title: Claude Code – Structured Code Review
purpose: Request a structured review of staged changes using Claude Code
recommended_tool: Claude Code CLI
context_requirements:
  - Branch and base commit
  - Diff or list of files changed
  - Tests executed and their results
  - Issue/PR link describing scope
token_budget_notes: Keep review prompts under 70k tokens by summarizing large diffs and focusing on critical files.
related_prompts:
  - code-review-codex-v1.0.md
  - partial-failure-followup-v1.0.md
  - test-failure-analysis-v1.0.md
  - refactor-extract-function-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial review template for Claude
---

## Template
```
Task: Review changes for <branch-name>
Base commit: <sha>
Issue/PR: <link>

Summary of changes:
<One paragraph describing the intent of the change>

Files changed:
- <file1>: <short note>
- <file2>: <short note>

Tests executed:
- <command> → <result>
- <command> → <result>

Questions / focus areas:
1. <Question or risk area>
2. <Question or risk area>

Instructions:
- Identify functional risks, missing tests, or doc gaps
- Call out any style or architectural inconsistencies
- Provide actionable suggestions with file references
- If comfortable, approve; otherwise flag blockers
```

## When to Use
- Before sending the change to human reviewers
- After significant refactors or multi-file edits

## When Not to Use
- Tiny changes that already passed review
- Changes requiring domain expertise outside Claude’s context

## Worked Example
```
Task: Review feature/extract-validators
Base commit: a1b2c3d
Issue/PR: #128 – "Extract payment validator"

Summary:
Validation logic was moved into src/validators/payment.js. Tests updated accordingly.

Files changed:
- src/billing.js: processPayment now calls validatePayment
- src/validators/payment.js: new helper
- tests/billing.test.js: imports updated

Tests:
- npm test -- billing.test.js → PASS

Questions:
1. Are there unhandled validation errors for missing fields?
2. Should we add telemetry when validation fails?

Instructions:
- Highlight risks, missing tests, doc updates
```

## Versioning Notes
- Update to v1.x when instructions or required data change (e.g., new sections for telemetry or doc review).
