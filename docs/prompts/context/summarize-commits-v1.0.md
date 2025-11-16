---
title: Summarize Commits for Prompt Context
purpose: Capture recent commit activity relevant to a prompt
recommended_tool: Any (prepares context for Claude, Codex, or reviewers)
context_requirements:
  - List of commit SHAs and subjects
  - Optional diff stats for each commit
  - High-level description of how commits relate to the task
token_budget_notes: Keep each commit summary under 400 tokens; limit to the last 5 relevant commits.
version_history:
  - v1.0 (2025-02-14): Initial commit summary template
---

## Template
```
Recent commits impacting this work:
1. <sha> – <subject> (files: <list>)
   Impact: <1-2 sentences>
2. <sha> – <subject>
   Impact: <1-2 sentences>
3. ...

Observed patterns:
- <mention regressions, risky areas, or repeated failures>

Use in prompts:
- Include this block when asking tools to consider recent changes
- Reference commit numbers instead of pasting large diffs
```

## Usage Notes
- Generate this summary during capture/refine stages.
- Store the block alongside the prompt log entry.
- Update after rebasing or when new commits land.

## Worked Example
```
Recent commits impacting this work:
1. a1b2c3d – "Extract validator skeleton" (files: src/billing.js)
   Impact: Added placeholder for external validator but logic still inline.
2. c9d1f23 – "Improve billing logging" (files: src/billing.js, src/logging.js)
   Impact: Adds structured logging that must remain after refactor.
3. 3ed92aa – "Add payment metrics" (files: src/metrics.js, src/billing.js)
   Impact: Introduced metrics hook inside processPayment(). Ensure it still fires after extraction.

Observed patterns:
- processPayment() has multiple responsibilities and is a frequent change hotspot.

Use in prompts:
- Attach this summary to Claude prompts when referencing historical context.
```

## Versioning Notes
- Bump version if new metadata (author, ticket links) becomes mandatory.
