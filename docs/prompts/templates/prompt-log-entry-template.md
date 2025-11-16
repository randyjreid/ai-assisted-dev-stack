---
title: Prompt Log Entry Template
purpose: Standard format for archiving prompts and their outcomes
storage_location: GitHub issue comments or PR discussions (primary); docs/prompts/logs/ (optional for significant prompts)
version_history:
  - v1.0 (2025-02-15): Initial template
---

## Prompt Log Entry Template

Use this template when archiving prompts in GitHub issues, PR discussions, or the optional logs directory.

### Template

```markdown
## Prompt Log Entry - [Short Description]

**Date**: YYYY-MM-DDTHH:MM:SSZ
**Tool**: Claude Code CLI / OpenAI Codex CLI / GitHub Copilot
**Model**: (e.g., Sonnet 4.5, GPT-4, etc.)
**Branch**: `branch-name`
**Issue/PR**: #<number> or <link>
**Files Affected**:
- `path/to/file1.js`
- `path/to/file2.js`

**Prompt Template Used**: (if applicable, e.g., `refactor-extract-function-v1.0.md`)

**Prompt** (sanitized):
```
[Full prompt text with credentials/PII removed]
```

**Outcome**: Success / Partial Success / Failed

**Result Summary**:
- [Brief description of what was produced]
- [Key changes or findings]
- [Any unexpected behaviors]

**Tests Run**:
- `npm test -- path/to/tests` → PASS/FAIL
- [Other validation commands]

**Follow-up Actions**:
- [ ] Task 1
- [ ] Task 2

**Lessons Learned**:
- [Optional: What worked well, what didn't, refinements for next time]

**Token Usage**: ~XXk tokens (optional tracking)
```

## Usage Notes

**When to create a log entry**:
- After significant prompts (multi-file changes, complex refactors)
- When the prompt or response will inform future work
- For prompts that failed (to document what went wrong)
- At the end of each major workflow stage

**Where to store**:
- **Primary**: GitHub issue comments or PR discussions (keeps context close to work)
- **Optional**: Copy to `docs/prompts/logs/YYYY-MM-DD-description.md` for long-term reference on significant prompts

**Sanitization reminder**:
Before archiving, remove:
- Credentials, API keys, tokens, passwords
- Personally identifiable information (PII)
- Internal system paths that reveal security information
- Sensitive business logic or trade secrets

## Example

```markdown
## Prompt Log Entry - Extract Payment Validator

**Date**: 2025-02-14T15:30:00Z
**Tool**: Claude Code CLI
**Model**: Sonnet 4.5
**Branch**: `feature/extract-validators`
**Issue/PR**: #128
**Files Affected**:
- `src/billing.js`
- `src/validators/payment.js`
- `tests/billing.test.js`

**Prompt Template Used**: `refactor-extract-function-v1.0.md`

**Prompt** (sanitized):
```
Task: Extract payment validation logic into a dedicated module
Branch: feature/extract-validators
Base commit: a1b2c3d

Files in scope:
- src/billing.js
- src/validators/payment.js
- tests/billing.test.js

Context:
processPayment() in src/billing.js currently performs validation inline (lines 45-78).
That logic needs to move into a new validatePayment() helper so we can reuse it elsewhere.
Existing tests cover the validation paths but live in tests/billing.test.js.

Requirements:
1. Create src/validators/payment.js exporting validatePayment(paymentData)
2. Move validation logic from billing.js lines 45-78 into the new module without changing behavior
3. Update processPayment() to call validatePayment()
4. Keep all existing tests passing; add imports or mocks as needed
5. Document the new helper with JSDoc comments

Constraints:
- Do not modify other functions in billing.js
- Do not change the public API of processPayment()
- Only edit the files listed above

Output format:
- Provide unified diffs for each file followed by a short summary of the changes
```

**Outcome**: Success

**Result Summary**:
- Created `src/validators/payment.js` with `validatePayment()` function
- Refactored `processPayment()` to call new validator
- All existing tests pass without modification
- Added JSDoc comments to new validator

**Tests Run**:
- `npm test -- tests/billing.test.js` → PASS (47/47)
- `npm run lint` → PASS

**Follow-up Actions**:
- [x] Code review by team
- [ ] Add integration tests for edge cases (scheduled for next sprint)

**Lessons Learned**:
- Including line ranges (45-78) in the prompt helped Claude extract precisely
- Explicit "do not modify processPayment API" constraint prevented scope creep
- Total token usage stayed under 50k by summarizing test file instead of including full content

**Token Usage**: ~48k tokens
```

## Versioning Notes
- Increment version when required fields or structure changes materially.
