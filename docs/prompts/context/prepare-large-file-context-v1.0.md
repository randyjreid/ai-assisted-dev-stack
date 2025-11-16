---
title: Prepare Large File Context
purpose: Provide a repeatable method for summarizing large files without exceeding token limits
recommended_tool: Any (context prep for Claude or Codex prompts)
context_requirements:
  - File path and size summary
  - Sections or line ranges relevant to the task
  - Optional AST or outline if available
token_budget_notes: Target 3k tokens maximum for the summary; rely on headings, bullet lists, and references rather than full code dumps.
related_prompts:
  - build-context-block-v1.0.md
  - summarize-commits-v1.0.md
  - refactor-extract-function-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial large-file context template
---

## Template
```
File: <path>
Size: <lines / kb>
Purpose: <short description>

Sections relevant to task:
1. <line range> – <description>
2. <line range> – <description>

Dependencies:
- <module/function> – <note>

Known risks:
- <list any tricky areas>

Reference snippet(s):
```<language>
<short excerpt>
```

Notes:
- Full file stored at <link or commit>
- Mention tests covering this file
```

## Usage Notes
- Build this summary before prompting when the source file exceeds token budgets.
- Store summaries in prompt logs so future prompts can reuse them.
- Update after major refactors.

## Worked Example
```
File: src/billing.js
Size: 420 lines (12 KB)
Purpose: Orchestrates payment flow

Sections:
1. Lines 1-80 – Input validation and logging
2. Lines 81-200 – Payment gateway integration
3. Lines 201-260 – Metrics and telemetry

Dependencies:
- src/validators/payment.js – (not yet extracted)
- src/logging/audit.js

Known risks:
- Validation logic intertwined with gateway calls

Snippet:
```js
function processPayment(payment) {
  if (!payment.cardNumber) {
    throw new Error('missing card');
  }
  // ...
}
```

Notes:
- Full file: https://github.com/.../src/billing.js@a1b2c3d
- Tests: tests/billing.test.js
```

## Versioning Notes
- Increment version if new sections (e.g., metrics) become required.
