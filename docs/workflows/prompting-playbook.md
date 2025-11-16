# Prompting Playbook

[\u2190 Back to Workflows Overview](../workflows.md)

This playbook operationalizes the principles from Section 2 of `docs/workflows.md`. It provides the concrete templates, patterns, and safeguards used to keep Claude Code, OpenAI Codex CLI, and GitHub Copilot aligned with the same intent and context during development.

## 2. Prompting and Interaction Patterns

### 2.1 Purpose of Prompting in This Stack
Prompting discipline is the control surface for AI assisted work. Structured prompts keep requests predictable, reproducible, and traceable so the team can understand how a change was produced even when model outputs vary. Every prompt ties back to the workflow lifecycle: capture the need (issue, roadmap entry, retro action), refine the instructions, execute with the right tool, evaluate the output with tests and reviews, then archive the transcript in the prompt log. Treating prompts like engineering artifacts makes the overall workflow auditable and lets future contributors replay or challenge earlier decisions.

### 2.2 Principles of Effective Prompting
**Clarity.** Each prompt states the scope, files in play, intent, acceptance criteria, and any guardrails. Explicit instructions such as “Edit `src/api/payments.js` only” or “Return diffs plus a summary” eliminate guesswork. Vague directives are rewritten before they reach the tools.

**Reproducibility.** Prompts reference the branch (`feature/add-payment-validator`), commit hash when relevant, and any prerequisite outputs. Even though model responses remain non-deterministic and will vary across runs, another developer can reproduce the process by reusing the same input conditions (branch, context, constraints). Reproducibility comes from the structured approach and human validation at each stage, not from identical AI outputs.

**Transparency.** Prompts link to the initiating artifact (GitHub issue, PR, design note) and explain why the work matters. Reviewers can follow those links to confirm context.

**Safety.** Prompts never include credentials, API keys, or personally identifiable information. Logs and stack traces are sanitized before inclusion. If sensitive data slips through, the comment or log entry is edited or removed immediately and noted in the security log.

**Verifiability.** Prompts request evidence: diffs, updated tests, or reasoning steps. When asking for analysis, the prompt demands structured findings (“List risks with severity and mitigation”). This keeps the human reviewer in control.

### 2.3 Shared Context Strategy
Context is assembled before any tool is invoked. The developer collects:
- Relevant file excerpts or snippets
- Recent commit summaries or diffs
- Issue or PR descriptions that define the goal
- Notes from design docs or architecture diagrams
- The latest entry from the prompt log for continuity

**Prompt logs** act as the permanent record. Each entry contains the prompt text, tool used, timestamp, branch, files touched, and a short summary of the result. Logs usually live in the corresponding GitHub issue or PR comment thread; for larger efforts they may also be copied into `docs/prompts/logs/` for long-term reference. Prompt libraries, by contrast, hold reusable templates and live under `docs/prompts/`, organized by workflow stage (implementation/, review/, context/, error-handling/, examples/).

Context can be reused when the task is still active and the code base has not drifted. Reset the context when:
- A different issue or feature is being addressed
- Significant refactors landed in main or the working branch
- The AI output references unstated files or assumptions (context drift)
- Token usage becomes excessive

**Token guidance.** Claude Code supports large contexts (200k tokens for Sonnet 4.5 as of January 2025). For best responsiveness, prioritize relevance and recency over exhaustiveness. Modern OpenAI models support 128k+ tokens; consult current model documentation for specific limits. Trim context by summarizing large files, pointing to commit hashes instead of pasting entire diffs, or delegating rarely needed details to follow-up prompts. See Section 2.3.1 for detailed context prioritization techniques.

### 2.4 Claude Code Prompt Patterns
Claude Code excels at structured, multi file changes. Every prompt follows this template:
1. **Task summary** – One sentence describing the outcome.
2. **Files in scope** – Explicit list of files or directories that may be edited.
3. **Context block** – Snippets, diffs, or design notes.
4. **Requirements** – Numbered list of functional expectations and tests to update or run.
5. **Constraints** – Forbidden files, API guarantees, safety reminders.
6. **Output format** – Diffs, patch file, or step-by-step plan.

Choose Claude Code when coordinating edits across files, performing complex refactors, or generating documentation updates that touch multiple directories. If a prompt approaches Claude’s token limit, fall back to a two-step process: first ask for a plan referencing file names only, then run smaller prompts per file.

**Example prompt.**
```
Task: Extract payment validation logic into a dedicated module
Branch: feature/extract-validators
Base commit: a1b2c3d

Files in scope:
- src/billing.js (edit)
- src/validators/payment.js (create)
- tests/billing.test.js (edit)

Context:
processPayment() in src/billing.js currently performs validation inline (lines 45-78). That logic needs to move into a new validatePayment() helper so we can reuse it elsewhere. Existing tests cover the validation paths but live in tests/billing.test.js.

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

### 2.5 Codex Prompt Patterns
Codex acts as an independent reviewer and analyst. Prompts are structured to encourage critical thinking rather than code generation. Typical layout:
1. **Context summary** – Issue link, feature description, or architectural concern.
2. **Diff or log excerpt** – What changed or what failed.
3. **Questions** – Concrete asks such as “What risks do you see?” or “Which tests should we add?”

Use Codex after Claude produces a change or when a second opinion is needed. Example:
```
Tool: OpenAI Codex CLI
Branch: feature/extract-validators
PR: #128 — "Extract payment validator"

Context:
Claude moved validation logic from src/billing.js into a new src/validators/payment.js helper. Tests still pass locally. We want an independent review before opening the PR.

Diff excerpt:
- src/billing.js: validation logic removed, now calls validatePayment()
- src/validators/payment.js: new helper with basic checks
- tests/billing.test.js: imports updated

Questions:
1. Are there edge cases or failure modes this refactor might miss?
2. Do we need additional tests to cover invalid payment data?
3. Are there security or logging implications of moving validation out of billing.js?

Please respond with:
- List of identified risks (if any)
- Recommended tests or safeguards
- Any documentation updates you would require before merge
```

This framing keeps Codex in reviewer mode and prevents it from attempting direct edits.

### 2.6 Copilot Interaction Model
Copilot supports the inner loop: inline suggestions, scaffolding, and lightweight refactors in the active editor. Use Copilot to propose small functions, test stubs, or repetitive boilerplate. When a Copilot snippet becomes part of a broader change, note its origin in the commit message or prompt log so reviewers know the context that led to the implementation. Switch back to Claude or Codex when a change spans multiple files, needs strict constraints, or requires analysis beyond the current buffer. If Copilot begins offering irrelevant suggestions, temporarily disable it for that file and focus on structured prompting instead.

### 2.7 Prompt Lifecycle
Prompts follow a consistent lifecycle:
1. **Capture** – Write down the problem statement, issue link, branch, and initial context. Drafts often start in a markdown snippet or issue comment template.
2. **Refine** – Review the prompt for clarity and completeness. Add acceptance criteria, constraints, or test expectations. High-risk prompts can be peer-reviewed before execution.
3. **Execute** – Run the prompt through Claude or Codex. Capture the full prompt text, the tool’s response, and any commands run (e.g., `claude apply`).
4. **Evaluate** – Validate the output with tests, linting, and manual review. If results are incomplete, branch the prompt history and iterate with refined instructions.
5. **Archive** – Store the final prompt and outcome in the prompt log, linking to summary commits or PRs. Logs should include timestamps, tool versions, and follow-up work items.

Prompt versioning lives alongside the templates in `docs/prompts/`. Each template has a version header (`Version: 1.2 – 2025-02-10 – Added security section`). When a template changes, update the history block so teams can revert to prior versions if needed.

### 2.8 Error Handling and Recovery

When AI responses fail to meet requirements or produce unexpected results, follow a systematic error handling approach rather than repeatedly retrying the same prompt.

#### 2.8.1 Common Failure Modes

**Ambiguous responses**: When a response lacks specificity or seems uncertain, ask the tool to:
- Restate its understanding of the task
- List the assumptions it made
- Identify which parts of the response are confident vs. uncertain

**Unintended edits**: If Claude or Codex modifies files outside the allowed scope:
- Instruct it to explain its reasoning for those edits
- Restate the explicit file list constraints
- If drift continues, use the `diagnose-drift-or-hallucination` template

**Partial failures**: When some requirements are met but others are incomplete:
- Record completed tasks in the prompt log
- Use the `partial-failure-followup` template to address outstanding items
- Do not restart from scratch unless the partial result is fundamentally flawed

**Hallucinations**: When the AI references nonexistent files, features, or assumptions:
- Use the `diagnose-drift-or-hallucination` template to understand the source
- Reset context if drift is detected
- Rebuild context from latest source of truth (branch, commit, docs)

#### 2.8.2 Retry Strategy

**First attempt fails**:
1. Review the prompt for ambiguity or missing context
2. Check if constraints were clear and explicit
3. Refine and retry once

**Second attempt fails**:
1. Use self-correction prompt (see below)
2. If still unclear, switch to analysis mode (ask for plan, not implementation)
3. Consider breaking task into smaller prompts

**Third attempt fails**:
- Escalate to different tool or human review
- Do not retry more than 3 times without changing approach

#### 2.8.3 Escalation Criteria

Escalate to a different tool when:
- **Multiple retries produce conflicting output** → Try Codex for second opinion
- **Problem shifts from implementation to analysis** → Switch Claude → Codex
- **Change is too granular for AI assistance** → Fall back to manual edits
- **Safety concerns arise** → Stop and consult security review

Escalate to human review when:
- AI consistently misunderstands requirements after 3 attempts
- Output quality degrades across iterations (sign of context pollution)
- Task involves sensitive architecture decisions beyond AI scope
- Regulatory, security, or legal considerations are present

#### 2.8.4 Self-Correction Prompts

Self-correction prompts are part of the standard toolkit. Use before finalizing any significant change:

```markdown
Before finalizing, review your proposal against the requirements above. For each requirement:
1. State whether it is fully satisfied (with evidence from your output)
2. Identify any requirements that are partially met or missing
3. List assumptions you made that need confirmation

If any requirements are not fully met, state what remains and stop. Do not proceed with implementation until gaps are addressed.
```

This meta-cognitive check catches issues before they reach code review.

#### 2.8.5 Logging Failures

Failed prompts are as valuable as successful ones for continuous improvement. Log failures with:
- The prompt that failed
- The problematic response or behavior
- Root cause analysis (ambiguous prompt, insufficient context, wrong tool, etc.)
- What approach succeeded instead

Store failure logs in the same location as success logs (GitHub issue comments or `docs/prompts/logs/`). Tag with `failure` or `retry` for easy filtering.

#### 2.8.6 Security Note

Prompts and their archived outputs must never contain credentials, API keys, or personally identifiable information. Before including logs, test output, or error messages in prompts:
- **Sanitize**: Remove credentials, tokens, PII, internal paths
- **Validate**: Double-check that no sensitive data remains
- **Flag**: If sensitive data appears in AI output, edit or delete the prompt log immediately and note the incident

Refer to the safety constraints in Section 2.2 and the prompt templates in `docs/prompts/implementation/` for specific guidance.

#### 2.8.7 Error Handling Templates

The prompt library provides three templates for error handling workflows:
- `error-handling/partial-failure-followup-v1.0.md` - Address incomplete implementations
- `error-handling/diagnose-drift-or-hallucination-v1.0.md` - Debug unexpected references
- `error-handling/request-self-correction-v1.0.md` - Validate AI output before proceeding

Consult these templates when standard prompts fail or produce unexpected results.

### 2.9 Playbook Integration
This playbook is the companion to Section 2 in `docs/workflows.md`. That document explains why prompting discipline matters; this file shows how to execute it. Other workflow playbooks (planning, implementation, testing, review) reference specific sections here when they require prompt templates or context handling procedures. As new patterns emerge, update the appropriate subsection here and link back from the corresponding workflow stage so the entire stack stays synchronized.

---

## Related Playbooks
- [Planning and Requirements Workflow](planning-and-requirements.md)
- [Implementation and Testing Workflow](implementation-and-testing.md)
- [Documentation Updates Workflow](documentation-updates.md)
- [Review and Merge Workflow](review-and-merge.md)
