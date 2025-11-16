---
title: Example – Codex Review Prompt
purpose: Show how to request an independent review from Codex
recommended_tool: OpenAI Codex CLI
context_requirements:
  - PR #128 summary
  - Diff snippets for src/billing.js and src/validators/payment.js
  - Test command output
  - Questions regarding risks and tests
token_budget_notes: 8k tokens (short diff excerpts only)
related_prompts:
  - code-review-codex-v1.0.md
  - code-review-claude-v1.0.md
  - build-context-block-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial example
---

## Prompt
```
Tool: OpenAI Codex CLI
Branch: feature/extract-validators
PR: #128 "Extract payment validator"

Context:
Validation logic was moved from src/billing.js into src/validators/payment.js. Tests/billing.test.js still pass. We want a second opinion on risks.

Diff highlights:
- billing.js: processPayment now calls validatePayment()
- validators/payment.js: new helper returning { valid, errors }

Tests executed:
- npm test -- billing.test.js → PASS

Questions:
1. Any missing validation edge cases?
2. Should telemetry/logging be updated after the refactor?
3. Are there security implications of moving validation out of billing.js?

Please respond with:
- Risks (with severity)
- Suggested tests or safeguards
- Documentation updates, if any
```

## Codex Response Summary
- Flagged missing checks for null paymentData
- Recommended tests for invalid currency input
- Suggested adding audit logs when validation fails

## Lessons Learned
- Tight focus on risks kept the response concise
- Including test results reassured Codex that existing checks still pass
