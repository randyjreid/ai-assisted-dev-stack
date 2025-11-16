---
title: Diagnose Context Drift or Hallucination
purpose: Help Claude or Codex explain unexpected references or edits
recommended_tool: Claude Code CLI or OpenAI Codex CLI
context_requirements:
  - Original prompt summary
  - Output that appears incorrect or unrelated
  - Current context block (files, issue link)
token_budget_notes: Keep to 25k tokens; focus on the mismatched portions rather than the entire prompt.
version_history:
  - v1.0 (2025-02-14): Initial drift diagnosis template
---

## Template
```
Original intent:
<one paragraph>

Observed issue:
- The response referenced <file/assumption> which was never provided
- Unexpected edit: <describe>

Task for the model:
1. Explain what context led you to reference <file/assumption>
2. Identify whether that context came from the current prompt, prior history, or your internal heuristics
3. Suggest how to realign the prompt (e.g., restate scope, reset session)

If you cannot justify the reference, acknowledge it and provide steps to avoid similar drift.
```

## When to Use
- AI output references nonexistent files or features
- Claude edits files outside the allowed list

## When Not to Use
- Legitimate misunderstandings caused by ambiguous requirements (clarify requirements instead)

## Worked Example
```
Original intent: Move validation logic in billing.js
Observed issue: Claude referenced src/payments/newFlow.js, which was never mentioned
Task: Explain why that file appeared and suggest alignment steps
```

## Versioning Notes
- Update when additional diagnostics (token history, command logs) become required.
