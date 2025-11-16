---
title: Bug Fix – Root Cause Analysis and Patch Plan
purpose: Help Claude Code analyze a bug report, identify root cause, and propose a safe fix plan
recommended_tool: Claude Code CLI
context_requirements:
  - Bug report or issue link with reproduction steps
  - Logs, failing test snippets, or stack traces (sanitized)
  - Relevant file excerpts
  - Current branch/commit information
token_budget_notes: Aim for 80k tokens or less. Include only the portion of logs or stack traces necessary for diagnosis.
related_prompts:
  - test-failure-analysis-v1.0.md
  - partial-failure-followup-v1.0.md
  - add-feature-boilerplate-v1.0.md
  - refactor-extract-function-v1.0.md
version_history:
  - v1.0 (2025-02-14): Initial bug analysis template
---

## Template
```
Task: Investigate and fix bug <bug-id>
Branch: <branch-name>
Base commit: <sha>

Bug summary:
<Describe user impact, reproduction steps, and severity.>

Logs / stack traces:
<Insert sanitized snippets or point to log files with line numbers>

Files to inspect:
- <path/to/file1>
- <path/to/file2>

Expectations:
1. Explain likely root cause(s) based on the context above
2. Propose a fix strategy referencing specific functions or modules
3. Outline the code changes that will be needed (no modifications yet)
4. Identify the tests to create or update
5. Flag any open questions or risks

Constraints:
- Do not modify files yet; produce analysis plus plan
- Highlight assumptions clearly
- If more data is needed, list it explicitly instead of guessing

Output format:
- Root cause analysis summary
- Proposed patch plan (bullet list)
- Test plan
- Open questions / risks
```

## When to Use
- Before making risky bug fixes that require careful analysis
- When debugging requires reviewing multiple files and logs

## When Not to Use
- Obvious one-line fixes that can be handled directly
- Incidents already escalated to another team

## Worked Example
```
Task: Investigate bug BUG-512 "Invoices missing line items"
Branch: bugfix/invoice-line-items
Base commit: f0e12cd

Bug summary:
Some invoices generated between Feb 10-11 omit line items when discounts are applied. Users see empty invoice tables but charges still go through. Severity high.

Logs / stack traces:
- invoices-service.log lines 223-240 show `TypeError: cannot read property 'map' of undefined`

Files to inspect:
- services/invoices/generator.js
- utils/discounts.js

Expectations:
1. Explain the likely root cause
2. Propose a fix strategy referencing generator.js functions
3. Outline code changes and tests
4. List risks or open questions

Constraints:
- Do not edit files yet

Output format:
- Analysis summary
- Patch plan
- Test plan
- Risks/questions
```

## Versioning Notes
- Increment the version when structure or output expectations change.
