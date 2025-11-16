---
title: Codex – Independent Code Review
purpose: Use OpenAI Codex CLI for second-opinion reviews and risk analysis
recommended_tool: OpenAI Codex CLI
context_requirements:
  - Short summary of change with issue/PR link
  - Diff excerpts or key file summaries
  - Tests executed and outstanding concerns
  - Any architectural notes relevant to the change
token_budget_notes: Keep total prompt under 12k tokens. Provide targeted diffs rather than entire files.
version_history:
  - v1.0 (2025-02-14): Initial Codex review template
---

## Template
```
Tool: OpenAI Codex CLI
Branch: <branch-name>
PR/Issue: <link>

Context:
<Brief description of the change, why it was made, and critical areas>

Diff highlights:
- <file>: <short description or snippet>
- <file>: <short description or snippet>

Tests executed:
- <command> → <result>

Questions for Codex:
1. List any functional or security risks you see
2. Suggest missing tests or documentation updates
3. Flag any design inconsistencies or unclear naming

Please respond with:
- Risks (with severity)
- Recommended tests or safeguards
- Documentation or follow-up actions
```

## When to Use
- After Claude review to gain a second opinion
- When human reviewers want additional assurance before merge

## When Not to Use
- Extremely small fixes
- Sensitive changes where sharing context outside the team is restricted

## Worked Example
```
Tool: OpenAI Codex CLI
Branch: feature/extract-validators
PR: #128

Context:
Validation logic was extracted into src/validators/payment.js so other payment flows can reuse it.

Diff highlights:
- billing.js: processPayment now calls validatePayment()
- validators/payment.js: new helper with error collection

Tests:
- npm test -- billing.test.js → PASS

Questions:
1. Any missing edge-case tests?
2. Do we need telemetry/logging updates?
3. Are there security implications of moving validation?
```

## Versioning Notes
- Increment the version when question sets or output expectations change.
