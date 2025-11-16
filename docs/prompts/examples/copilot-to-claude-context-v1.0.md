---
title: Example – Copilot to Claude Context Handoff
purpose: Show how to capture Copilot-generated scaffolding and feed it into a Claude prompt
recommended_tool: Copilot (initial), Claude Code CLI (follow-up)
context_requirements:
  - Copilot snippet details
  - Branch info and files touched
  - Prompt log references for continuity
  - Planned Claude task
token_budget_notes: Context block under 5k tokens; Claude prompt under 60k.
related_prompts:
  - build-context-block-v1.0.md
  - add-feature-boilerplate-v1.0.md
  - refactor-extract-function-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial handoff example
---

## Copilot Phase
- Branch: feature/extract-validators
- File: src/validators/payment.js
- Copilot suggestion:
```js
export function validatePayment(payment) {
  const errors = [];
  if (!payment.cardNumber) {
    errors.push('missing card');
  }
  return {
    valid: errors.length === 0,
    errors,
  };
}
```
- Notes: Copilot provided a starting structure but missed expiry/cvv checks.
- Logged in prompt log entry #128-00 with comment "Copilot scaffold for validatePayment".

## Claude Follow-Up Prompt
```
Task: Complete validatePayment(payment) implementation
Branch: feature/extract-validators
Base commit: a1b2c3d

Files in scope:
- src/validators/payment.js
- tests/billing.test.js

Context:
Copilot created a skeleton for validatePayment() (see snippet above). Need to add full validation logic currently located in billing.js lines 45-78 and update tests as needed.

Requirements:
1. Add expiry and CVV checks with descriptive error messages
2. Ensure validatePayment() returns { valid, errors }
3. Update tests to import the helper and cover all failure paths
4. Keep existing billing.js behavior unchanged

Constraints:
- Only edit listed files
- Do not change processPayment() API

Validation:
- Run npm test -- billing.test.js
- Provide diffs with short summary
```

## Lessons Learned
- Capturing the Copilot snippet in the prompt log provided traceability
- Claude understood exactly what remained because the Copilot context was explicit
- Token usage stayed low by referencing the snippet rather than pasting entire files
