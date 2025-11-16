---
title: Example – Claude Code Refactor Prompt
purpose: Demonstrate a complete refactor prompt using the extract-function template
recommended_tool: Claude Code CLI
context_requirements:
  - Issue #451 "Extract payment validation"
  - Branch feature/extract-validators, base a1b2c3d
  - Files src/billing.js, src/validators/payment.js, tests/billing.test.js
  - Prompt log reference #128-02
token_budget_notes: 30k tokens (small snippet). The real prompt stayed under 50k.
version_history:
  - v1.0 (2025-02-14): Initial example
---

## Prompt
```
Task: Extract payment validation logic into a dedicated module
Branch: feature/extract-validators
Base commit: a1b2c3d

Files in scope:
- src/billing.js
- src/validators/payment.js
- tests/billing.test.js

Context:
processPayment() currently validates card number, expiry, and CVV inline (lines 45-78). We need validatePayment(paymentData) to live in src/validators/payment.js so other services can reuse it. tests/billing.test.js already covers success/failure cases.

Requirements:
1. Create validatePayment(paymentData) that returns { valid, errors }
2. Move logic from billing.js lines 45-78 into the helper
3. Update processPayment() to call validatePayment()
4. Ensure tests/billing.test.js pass
5. Document the helper with JSDoc

Constraints:
- Only edit the files listed above
- Do not change processPayment() arguments or return value

Validation:
- Run npm test -- billing.test.js
- Provide diffs with summaries
```

## Result Summary
- Claude created src/validators/payment.js with the extracted logic
- processPayment() now handles the helper response
- Tests were untouched but still pass; developer reran npm test locally
- Prompt log entry archived in GitHub issue #128 comments (or optionally in `docs/prompts/logs/2025-02-14-claude-refactor.md` for long-term reference)

## Lessons Learned
- Including test commands in the validation block reminded Claude to keep them intact
- Explicit constraints prevented modifications to unrelated billing code
