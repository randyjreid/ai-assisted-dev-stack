---
title: Build Context Block for Multi-Tool Prompts
purpose: Provide a reusable format for capturing issue, file, and commit context before prompting
recommended_tool: Works with Claude Code, Codex, or Copilot handoffs
context_requirements:
  - Issue or PR link
  - Current branch and base commit
  - Files involved and their roles
  - Recent commits affecting the same area
token_budget_notes: Aim for 5k tokens or less; this block should be concise enough to prepend to larger prompts.
version_history:
  - v1.0 (2025-02-14): Initial context template
---

## Template
```
Context summary:
- Issue/PR: <link>
- Branch: <branch-name> (base <sha>)
- Goal: <one-sentence problem statement>
- Recent commits: <sha1 short desc>; <sha2 short desc>

Files involved:
1. <file path> – <role>
2. <file path> – <role>

Key snippets:
```<language>
<insert minimal code excerpt or pseudocode>
```

Test status:
- <command> → <result>

Prompt history reference:
- Last prompt log entry: <link or timestamp>
```

## Usage Notes
- Assemble this block during the capture stage of the prompt lifecycle.
- Store each completed block in the prompt log so future prompts can reuse it.
- When the code base diverges, refresh the branch, re-run tests, and rebuild the block.

## Worked Example
```
Context summary:
- Issue: #451 "Extract payment validation logic"
- Branch: feature/extract-validators (base a1b2c3d)
- Goal: Move inline validation into reusable helper
- Recent commits: c9d1f23 "Improve billing logging"; 3ed92aa "Add payment metrics"

Files involved:
1. src/billing.js – Houses processPayment()
2. src/validators/payment.js – New helper

Key snippets:
```js
function processPayment(payment) {
  // validation lines 45-78
}
```

Test status:
- npm test -- billing.test.js → PASS

Prompt history reference:
- Prompt log #128-01
```

## Versioning Notes
- Increase version when structure changes or new metadata is required.
