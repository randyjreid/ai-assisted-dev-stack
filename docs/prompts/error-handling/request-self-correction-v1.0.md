---
title: Request Self-Correction from AI Tool
purpose: Prompt Claude or Codex to review its own proposal for gaps before proceeding
recommended_tool: Claude Code CLI or OpenAI Codex CLI
context_requirements:
  - Previous prompt text and response summary
  - Requirements list for comparison
  - Known tests or validation steps
token_budget_notes: Keep under 20k tokens; include only the essential requirements and response excerpts.
version_history:
  - v1.0 (2025-02-14): Initial self-correction template
---

## Template
```
Context:
- Previous prompt: <title/link>
- Summary of AI response: <short description>
- Requirements:
  1. <req1>
  2. <req2>
  3. <req3>

Task:
Review your previous response and compare it against the requirements above. List:
1. Requirements fully satisfied (with evidence)
2. Requirements partially satisfied or missing (explain gaps)
3. Any assumptions you made that need confirmation

If gaps exist, propose precise follow-up actions instead of guessing.
```

## When to Use
- After receiving a large patch or plan from Claude/Codex
- Before running costly tests or reviews

## When Not to Use
- When requirements are ambiguous (clarify first)
- For trivial prompts where manual review is faster

## Worked Example
```
Context:
- Previous prompt: "Extract payment validator"
- Response summary: Claude created validatePayment() but skipped tests
- Requirements: create helper, update processPayment(), keep tests passing, add docs

Task:
Review response vs requirements; list satisfied items, missing items, assumptions
```

## Versioning Notes
- Bump version when additional sections (e.g., metrics) become necessary.
