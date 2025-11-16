---
title: Partial Failure Follow-Up
purpose: Provide a structured follow-up when a prompt succeeds partially but leaves tasks unfinished
recommended_tool: Claude Code CLI
context_requirements:
  - Previous prompt and response
  - List of completed vs pending tasks
  - Updated file list or diffs
token_budget_notes: Keep under 40k tokens; focus on the unresolved portions.
related_prompts:
  - diagnose-drift-or-hallucination-v1.0.md
  - request-self-correction-v1.0.md
  - code-review-claude-v1.0.md
  - refactor-extract-function-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial follow-up template
---

## Template
```
Previous prompt summary:
<short paragraph>

Completed tasks:
- <task/diff>

Outstanding tasks:
1. <task>
2. <task>

New instructions:
- Address only the outstanding tasks
- Do not modify completed sections unless required to finish the remaining work
- Provide updated diffs and note any side effects

Validation:
- Rerun <tests>
- Highlight files touched in this follow-up
```

## When to Use
- Claude finished some requirements but skipped others
- The change needs incremental polish rather than a full restart

## When Not to Use
- When the original prompt was flawed; rewrite instead
- When too many files changed unexpectedly (reset context first)

## Worked Example
```
Previous prompt: Add rate limit toggle
Completed: Backend behavior updated
Outstanding: UI checkbox not wired; docs not updated
Instructions: Focus on UI + docs only, leave backend untouched
Validation: npm test -- ui, verify docs rendered
```

## Versioning Notes
- Increment when validation or instruction blocks change materially.
