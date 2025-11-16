---
title: Refactor – Extract Function
purpose: Guide Claude Code through extracting logic into a dedicated helper while preserving behavior
recommended_tool: Claude Code CLI
context_requirements:
  - Current branch name and base commit hash
  - File excerpts showing the logic to extract
  - Related tests or documentation sections
  - Issue or PR links describing why the refactor is needed
token_budget_notes: Keep total prompt focused and relevant. For Sonnet 4.5, aim for under 100k tokens for best responsiveness. Summarize large files instead of pasting entire modules; reference commit IDs for full diffs.
related_prompts:
  - add-feature-boilerplate-v1.0.md
  - code-review-claude-v1.0.md
  - partial-failure-followup-v1.0.md
  - test-failure-analysis-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial template for extraction refactors
---

## Template
```
Task: Extract <functionality> into a dedicated helper
Branch: <branch-name>
Base commit: <sha>

Files in scope:
- <path/to/source-file> (edit)
- <path/to/new-helper-file> (create|edit)
- <path/to/tests> (edit)

Context:
<Explain current structure, include key code snippets or line ranges that need extraction. Mention existing tests that will exercise the new helper.>

Requirements:
1. Create <new helper> with clear parameters and return values
2. Move the logic from <source file line range> into the helper without changing behavior
3. Update <call site(s)> to use the helper
4. Ensure tests in <test file> continue to pass; update imports or mocks if necessary
5. Add documentation or inline comments describing the helper

Constraints:
- Only modify the files listed above
- Preserve public APIs unless explicitly stated
- Do not change unrelated functionality
- If assumptions are unclear, pause and ask for clarification
- Do not include credentials, API keys, or sensitive data in prompts or outputs
- Sanitize any logs or error messages before inclusion

Validation:
- Report commands needed to run unit tests and lint
- Provide unified diffs for each file followed by a summary of the changes
```

## When to Use
- Coordinated multi-file refactors where logic needs to be extracted or deduplicated
- When tests already cover the affected code paths
- When the refactor must preserve behavior exactly

## When Not to Use
- Net-new feature development (use the add-feature template instead)
- Simple single-file edits that are faster to do manually
- Large architectural changes that require design approval first

## Worked Example
```
Task: Extract payment validation logic into a helper
Branch: feature/extract-validators
Base commit: a1b2c3d

Files in scope:
- src/billing.js
- src/validators/payment.js
- tests/billing.test.js

Context:
processPayment() currently validates card number, expiry, and CVV inline (lines 45-78). These checks will be moved into validatePayment() in src/validators/payment.js so other flows can reuse them. tests/billing.test.js already covers success and failure cases.

Requirements:
1. Create validatePayment(paymentData) that returns an object { valid, errors }
2. Move validation logic from billing.js lines 45-78 into the helper without changing behavior
3. Update processPayment() to call validatePayment() and handle errors accordingly
4. Update tests to import the new helper if needed
5. Document validatePayment() with JSDoc

Constraints:
- Only edit the listed files
- Keep processPayment() public API unchanged
- Surface any assumptions before modifying business logic
- Do not include credentials, API keys, or sensitive data in prompts or outputs
- Sanitize any logs or error messages before inclusion

Validation:
- Run npm test -- billing.test.js
- Provide diffs for each file with a short summary
```

## Versioning Notes
- Increment the version (v1.x) whenever the structure of the template or validation steps change.
- Document the reason in the version_history block.
