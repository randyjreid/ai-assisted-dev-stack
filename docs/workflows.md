# Workflows

This document describes the complete lifecycle for the AI Assisted Dev Stack, from the first idea to the moment a change is merged and validated. It shows how prompting, planning, design, implementation, testing, review, documentation, and continuous improvement fit together, with explicit touchpoints for Claude Code, Copilot, Codex, and the rest of the toolchain. Detailed, step by step instructions for each stage will be captured in the playbooks under `docs/workflows/`.

## 1. Workflow Overview

### 1.1 Purpose of These Workflows
The workflow system exists to make the development process predictable, auditable, and aligned with the stack conventions while still leaving space for experimentation. Predictability comes from defining explicit entry and exit criteria for each stage so work follows repeatable patterns and the team knows what "ready" looks like before moving forward. The AI tools used throughout this workflow (Claude Code, Copilot, and Codex) are non-deterministic: the same prompt can produce different outputs across runs. Predictability in this system is created by the structured process and human review at each stage, not by assuming deterministic AI output. Every AI-generated change requires human judgment and validation before it advances through the workflow. Auditability is achieved by keeping a clear chain of evidence: prompts point to issues, issues point to branches, branches point to pull requests, and decisions are captured in review threads, documentation, and workflow logs. Alignment with stack conventions means branching strategies tie back to roadmap items, pull request templates enforce review expectations, and documentation checkpoints cover the overview, components, install, and workflow guides whenever a change modifies behavior or process.

These workflows also provide controlled spaces for experimentation. Dedicated branches or sandboxes allow new prompt strategies, tool combinations, or automation scripts to be tested without risking the main branch. Successful experiments feed back into the standard workflow so good ideas become part of the baseline instead of one-off wins. Results are recorded in the update log or linked GitHub issues so future contributors can audit what was tried, why it worked, and how it was adopted. Each workflow stage is explicitly tied to the roadmap phases: Phase 0 established the scaffolding and baseline documentation, Phase 1 focuses on codifying the core stack documentation and workflow definitions, and later phases will introduce automation scripts, deeper workflow playbooks, public documentation, and an evolving toolchain.

### 1.2 High Level Lifecycle
The lifecycle begins with prompting and intent, where the goal is to capture the problem statement and the context required by AI tools. Ideas, issues, and user feedback act as the primary inputs, and the outputs are refined goals, initial prompts, and shared context that align with roadmap priorities. Planning and requirements then translate that intent into actionable work by producing task breakdowns, requirement updates, and acceptance criteria. This stage consumes prompting outcomes and existing documentation, and it can loop with the prompting stage when clarification is needed.

Design and architecture follows, defining the technical approach, interfaces, and constraints. It relies on the plans and requirements and produces design notes, diagrams, and decision records that guide the next stages. Implementation uses those artifacts to build the change in a branch through incremental commits, generating code updates, local tests, and status reports. Testing validates that work by running unit, integration, and functional checks, turning code changes and test plans into test results, bug reports, and readiness signals. Testing runs against stabilized commits that map to the unit and integration suites referenced in Section 6. While developers may run informal spot checks during active development, formal testing in the workflow begins after implementation has reached a stable point and the changes are ready for validation.

Review ensures the change meets quality expectations through a mix of AI-assisted and human analysis. Pull requests, diffs, test evidence, and documentation updates feed into this stage, which produces approvals, review notes, and follow-up tasks. Documentation updates ensure the overview, components, install, workflows, and playbooks stay synchronized with the code. Once documentation aligns with the latest implementation and review feedback, merge and post-merge validation integrate the change into the main branch, creating merged commits, tags, and validation records. Immediate sanity checks happen here, and any issues can trigger new planning or bug work.

The cycle closes with continuous improvement. Retro notes, validation outcomes, and roadmap signals are reviewed to refine prompts, adjust workflows, and plan tool upgrades. Insights captured in this phase flow directly into the next prompting and planning effort, so the lifecycle is both sequential and iterative. Each stage depends on the previous stage's outputs, yet feedback loops allow planning, design, implementation, and documentation to run in parallel when a change requires rapid iteration. These handoffs align with the roadmap checkpoints so each phase delivers a measurable outcome before the next begins.

### 1.3 Role of Tools Across the Lifecycle
Git and GitHub serve as the system of record across every stage. They host branches, commits, pull requests, issues, and roadmap entries, and they provide the links that tie documentation, prompts, and design notes back to a single history. GitHub CLI builds on that foundation by handling branch creation, status checks, pull request operations, and reviews directly from the terminal. It exposes the same data as the GitHub web interface, which makes it easy to share context with AI tools through commands and outputs, while leaving the IDE free for editing and testing.

Visual Studio Code is the integration point for design notes, implementation work, testing, and documentation updates. It houses the local files, WSL terminals, and extensions that connect Copilot, Claude, and Codex into a single workspace, but it is not the authoritative history. GitHub Copilot focuses on the inner loop during implementation, testing, and lightweight documentation edits by offering inline suggestions based on the currently open files, repository structure, recently edited files, and learned patterns. It is best suited for small changes, patterns, and quick scaffolding rather than repository-wide transformations.

Claude Code CLI provides structured, repository-aware assistance for planning refinement, design exploration, implementation tasks, documentation updates, and review support. It works with a large context window (on the order of hundreds of thousands of tokens) and uses smart selection strategies to load relevant files, recent changes, and prompt history into that context. This makes it ideal for coordinated edits that touch multiple files, though it relies on intelligent sampling rather than ingesting the entire repository at once. Every change still goes through human review before merge. OpenAI Codex CLI offers a complementary perspective during planning, validation, review, and continuous improvement. It is commonly used for second opinions, alternative solutions, or root-cause analysis, and it works best when prompts clearly describe the desired outcome and reference existing documentation.

### 1.4 Inputs and Outputs of the Workflow System
The workflow system consumes a steady stream of inputs: ideas from users or stakeholders, roadmap entries, bug reports, requirements, existing documentation, design artifacts, tool diagnostics, retrospective notes, and prompt libraries. Each input feeds into the lifecycle stage where it adds the most value. The outputs are merged code and tests that satisfy the requirements, updated documentation across all guides and playbooks, roadmap adjustments, version history entries, and refined workflow practices. Failure artifacts are also first-class outputs: failed test reports, rollback logs, and incident notes are captured, linked in GitHub issues or `update-log.md`, and fed into the continuous improvement cycle to refine future workflows and prevent recurring issues.

Section 2 onward dives deeper into each lifecycle stage and links to the related playbooks that explain the day-to-day execution. The inputs and outputs defined here describe how information flows between stages, tools, and documents, ensuring the later sections have a clear set of boundaries and expectations to build on.

## 2. Prompting and Interaction Patterns

Prompt discipline keeps multiple AI tools aligned with the same intent, context, and guardrails. This section outlines the shared principles, context strategy, and tool-specific patterns that make prompting reproducible while leaving room for human oversight. Detailed prompt templates, logging conventions, and examples will be developed in the [Prompting Playbook](workflows/prompting-playbook.md); the narrative below explains how those artifacts fit into the broader workflow.

**Note on OpenAI Codex CLI**: This document references "OpenAI Codex CLI" or "Codex" to refer to the OpenAI Codex command-line interface, an active and fully supported AI coding agent from OpenAI currently deployed in this project. Codex is used alongside Claude Code and GitHub Copilot to provide independent reviews, alternative design perspectives, and analytical insights throughout the workflow.

### 2.1 Introduction to Prompting Workflows
Prompting is treated like any other engineering workflow: capture the problem, gather context, run a structured process, and keep the results traceable. The aim is to keep Claude Code, Codex, and Copilot focused on the same objectives so the developer spends time reviewing and integrating rather than reinterpreting conflicting AI output. Disciplined prompting makes the stack faster because prompts become reusable building blocks, and safer because every interaction links back to the issue, branch, or documentation that justified the work.

### 2.2 Principles of Prompting
Clarity is paramount. Each request specifies a single outcome, the files in scope, the acceptance criteria, and any relevant constraints or safety instructions. Ambiguous requests such as "clean up this module" are replaced with concrete statements like "extract the validation logic from `processPayment()` in `src/billing.js` into a new `validatePayment()` function, preserve all existing test coverage, and update the inline documentation." To ensure reproducibility despite the non-deterministic nature of AI models, prompts include branch names, commit hashes when necessary, and references to previous outputs. This allows the same scenario to be re-run with consistent context, even though the model's responses may still vary due to non-determinism.

Transparency and auditability require prompts to cite their source. If a bug report triggered the work, the prompt links to that issue and explains the impact. When documentation informs the change, the prompt states which sections should be considered. Every significant prompt is stored in an issue comment, pull request discussion, or the prompt log (defined below) so reviewers can see how the AI was directed. Safety guardrails are embedded throughout: prompts explicitly list the files the AI may touch, discourage speculation, and remind the model to surface uncertainties rather than inventing answers. They also describe how the output will be validated, such as running a particular test suite or comparing behavior against the current design.

Verifiable output is the final principle. Prompts instruct the model to produce diffs, test updates, or step-by-step reasoning so humans can confirm the work. When a prompt requires analysis rather than code, it asks for structured findings and follow-up actions instead of generic commentary. Developers compare potential prompts before sending them, choosing the version that is easiest to verify later and noting poor examples in the playbook so the team can avoid them.

### 2.3 Shared Context Strategy
High-quality prompts start with curated context. Before invoking an AI tool, the developer gathers the issue or roadmap entry, links to the relevant documentation, important file excerpts, architecture diagrams, and any failing test logs. This context is summarized in the prompt itself and stored alongside the prompt text for documentation and for reuse when manually constructing prompts for other tools.

Context persists across sessions through several mechanisms:
- Saving reusable blocks in the playbook for common subsystems
- Reusing markdown snippets that describe architectural components
- Tagging prompt logs with the files, issues, and branches involved

When prompt history becomes tangled or conflicting instructions appear, the developer resets the session and rebuilds the context from the latest source of truth to prevent stale assumptions or partial histories from steering later prompts. Consider resetting in these situations:
- After large refactors that change file structure or module boundaries
- When switching between unrelated tasks
- When the AI output shows signs of drift (for example, referencing files that were never mentioned or making assumptions not grounded in the provided context)

**Prompt logs** are the primary archival mechanism for prompts and their outputs. They are stored as comments in GitHub issues or pull request discussions, tagged with issue numbers, branch names, and the files affected. This makes it easy for reviewers and future contributors to understand how AI-generated changes were produced. Prompt logs differ from prompt libraries (reusable templates) in that logs capture the full history of a specific interaction, while libraries contain generalized patterns for common tasks.

**Prompt library** is the collection of reusable prompt templates organized in `docs/prompts/`. The library is structured by workflow stage and purpose:

```
docs/prompts/
  implementation/
    refactor-extract-function-v1.0.md
    add-api-endpoint-v1.1.md
  review/
    code-review-checklist-v1.0.md
    security-review-v1.0.md
  analysis/
    technical-debt-assessment-v1.0.md
```

Each prompt template is a Markdown file with a simple header block containing the version, last updated date, and a brief description, followed by the prompt template with placeholders for project-specific details. Prompts are named descriptively with semantic versions (v1.0, v1.1, v2.0) in the filename. Git history provides the full evolution, while the version number allows teams to reference specific prompt iterations and roll back if needed.

Treating context as a first-class artifact makes it easier to share reasoning with reviewers and keeps each AI interaction grounded in the latest information.

#### 2.3.1 Managing Context Window Limits

Context windows are large but finite. Claude Code's context window holds hundreds of thousands of tokens (roughly 4 characters per token), which accommodates substantial context, but developers still need to prioritize when working with large repositories or complex changes.

When the desired context exceeds practical limits:

- **Prioritize recent and relevant changes**: Include files that were recently modified or are directly related to the current task. Older or tangential code can be summarized or omitted.
- **Include only necessary code blocks**: Instead of pasting entire files, include only the functions, classes, or sections that the AI needs to see. Add a brief comment describing the file's overall purpose.
- **Break large tasks into smaller prompts**: If a refactoring touches dozens of files, handle it incrementally. Prompt for a subset of changes, validate, then move to the next batch.
- **Summarize historical context**: If earlier work informs the current task, include a summary rather than full conversation history. For example: "In PR #123, we extracted validators to a separate module. This task extends that pattern to error handlers."

Testing prompt length before committing to a long interaction helps avoid mid-task context overflows. If a prompt seems too large, trim less critical details or split the work across multiple focused prompts.

### 2.4 Claude Code Prompt Patterns
Claude Code handles structured, repository-aware tasks, so prompts follow a consistent template:
1. **Summary**: The goal and current branch/commit
2. **Files in scope**: Explicit list of files that may be edited
3. **Context block**: Code snippets, diffs, or design notes
4. **Requirements and acceptance criteria**: What must change and which tests or commands prove success
5. **Constraints and safety directions**: What the model must not do

Coordinated multi-file edits spanning different directories rely on that structure to limit scope while still giving Claude access to relevant information. Implementation tasks spell out function-level behavior, input and output expectations, and testing hooks. Documentation prompts describe tone, link targets, and any cross-references to update. Refactoring prompts emphasize invariants to preserve and often require the model to describe the planned sequence before applying changes. Each prompt ends with explicit instructions about patch formatting and human review so changes enter the Git workflow cleanly.

**Example: Strong Claude Code prompt for refactoring**
```
Branch: feature/extract-validators
Commit: a1b2c3d
Goal: Extract payment validation logic into a dedicated module

Files in scope:
- src/billing.js (edit)
- src/validators/payment.js (create)
- tests/billing.test.js (edit)

Context:
The processPayment() function in src/billing.js currently mixes business logic with validation. Lines 45-78 perform validation checks that should be extracted into a separate validatePayment() function in a new src/validators/payment.js module.

Requirements:
1. Create src/validators/payment.js with a validatePayment(paymentData) function
2. Move validation logic from billing.js lines 45-78 to the new module
3. Update processPayment() to call the new validator
4. Preserve all existing test coverage in tests/billing.test.js
5. Add JSDoc comments to the new validator function

Acceptance criteria:
- All tests in tests/billing.test.js pass
- npm run lint reports no new errors
- The validatePayment function is exported and documented

Constraints:
- Do not modify any other billing logic outside lines 45-78
- Do not change the public API of processPayment()
- Do not remove or modify existing tests

Output format: Provide diffs for each file with clear explanations.
```

This example shows the level of specificity and structure that makes Claude Code prompts effective and verifiable.

### 2.5 Codex Prompt Patterns
Codex complements Claude by acting as an independent reviewer, strategist, or analyst. Prompts ask Codex to read diffs, rate pull requests, surface risks, or explore alternative designs rather than implementing changes directly. Independent review prompts supply the same context as a human reviewer would see—issue link, summary of changes, test evidence—and request actionable feedback or open questions. When exploring design options, the prompt shares the current approach and asks Codex to challenge assumptions or propose variants. Risk analysis prompts look for failure modes, security concerns, or missing instrumentation. Root-cause debugging prompts include logs, stack traces, or failing tests and ask for diagnostic steps rather than patches. Pull request scoring prompts request a readiness scorecard covering tests, documentation, and clarity.

The framing is deliberate. Prompts tell Codex to "act as a reviewer" or "provide a second opinion" so it stays in analyst mode. Codex is chosen when a second viewpoint or validation is needed; Claude remains the tool for scoped implementations. Switching tools unnecessarily introduces overlap, so the workflow calls for Codex only when its perspective adds value beyond what Claude and Copilot already produced.

**Example: Codex pull request review prompt**
```
Context: Pull Request #156 - Refactor authentication middleware
Issue: https://github.com/org/repo/issues/142
Branch: feature/auth-middleware-refactor
PR: https://github.com/org/repo/pull/156

Summary of changes:
- Extracted authentication logic from route handlers into middleware
- Added JWT token validation with expiry checks
- Implemented role-based access control (RBAC) helpers
- Updated 12 route handlers to use new middleware

Files changed:
- src/middleware/auth.js (new, 145 lines)
- src/middleware/rbac.js (new, 67 lines)
- src/routes/*.js (12 files modified)
- tests/middleware/auth.test.js (new, 89 lines)

Test results:
- Unit tests: 47/47 passing
- Integration tests: 12/12 passing
- Code coverage: 94% (up from 78%)

Your task:
Act as an independent code reviewer. Analyze this pull request for:

1. Security concerns:
   - Are JWT tokens validated correctly?
   - Are there timing attack vulnerabilities in token comparison?
   - Does RBAC properly prevent privilege escalation?
   - Are there any injection risks in the authentication flow?

2. Design quality:
   - Is the middleware pattern applied consistently?
   - Are there better alternatives to the current RBAC approach?
   - Does this refactoring introduce coupling or maintenance risks?

3. Missing coverage:
   - What edge cases might not be tested?
   - Are there failure modes that need additional instrumentation?
   - Should there be rate limiting or brute force protection?

4. Documentation and clarity:
   - Is the authentication flow clear to future maintainers?
   - Are error messages actionable for debugging?

Provide:
- Specific line-level concerns with file:line references
- Severity rating (critical/important/minor) for each issue
- Concrete suggestions for improvement
- Any questions about design decisions that need clarification

Do not provide implementation code. Focus on analysis and actionable feedback.
```

This example demonstrates how Codex prompts frame the request as analysis rather than implementation, provide full context including test results, and specify the exact type of feedback needed.

### 2.6 Copilot Interaction Model
Copilot thrives in the inner loop. Developers use it for inline suggestions, quick scaffolding, micro-refactors, and lightweight test stubs. These suggestions are treated as drafts: if a Copilot snippet becomes part of a broader change, the developer annotates the commit or prompt log so future readers understand the origin. This workflow reserves complex multi-file changes and architectural decisions for Claude Code, where structured prompts provide better safety and traceability.

Copilot and the broader prompting flow stay in sync by capturing context in editor comments or TODOs before switching to Claude or Codex. When Copilot provides a useful snippet, that snippet and the surrounding reasoning become part of the prompt history so the rest of the toolchain understands the current direction. If Copilot's suggestions stray from the plan or introduce noise, the developer disables it for that file and switches back to structured prompting until the work stabilizes. Copilot becomes the preferred option when speed matters more than structure, such as writing small utilities or filling in repetitive boilerplate.

**Example: Handoff from Copilot to Claude Code**

During implementation, Copilot suggests a helper function for data validation:

```javascript
// src/utils/validators.js
// AI-suggested: helper for sanitizing user input (Copilot, commit a7f3c2e)
function sanitizeInput(input) {
  return input.trim().replace(/[<>]/g, '');
}
```

The developer adds the comment noting the AI origin and commits it. Later, when creating a Claude Code prompt to expand the validation system, they reference this work:

```
Branch: feature/input-validation
Commit: a7f3c2e
Goal: Expand input validation system across all API endpoints

Context:
In commit a7f3c2e, Copilot suggested a basic sanitizeInput() helper in
src/utils/validators.js that handles HTML tag removal. This task extends
that pattern into a comprehensive validation framework.

Files in scope:
- src/utils/validators.js (expand existing)
- src/middleware/validation.js (create)
- src/api/routes/*.js (update to use new validators)

Requirements:
1. Extend sanitizeInput() to handle SQL injection patterns and script tags
2. Create validateEmail(), validateURL(), validatePhone() functions
3. Build a validation middleware that applies sanitization before route handlers
4. Update all POST/PUT routes to use the validation middleware
...
```

This approach preserves the Copilot contribution in the commit history and integrates it into the structured prompt, ensuring continuity across tools.

### 2.7 Prompt Lifecycle
Prompts move through the same lifecycle as code changes:

1. **Capture**: Record the intent, source issue, and initial context
2. **Refine**: Tighten wording, add constraints, and include a quick peer review for high-risk work
3. **Execute**: Interact with Claude or Codex and save the prompt and output together
4. **Evaluate**: Run tests, review diffs, and decide whether a follow-up prompt is required
5. **Archive**: Store the prompt, the resulting change, and any lessons learned in the issue tracker, pull request discussion, or prompt log

Maintaining this lifecycle keeps prompts auditable. Reviewers can trace a commit back to the prompt that produced it, see the context that was supplied, and understand why a particular approach was chosen. When prompts fail, the archive shows what went wrong and informs better instructions next time.

**Prompt versioning**: As workflows evolve, successful prompts are versioned in the prompt library with timestamps and notes about when and why they were updated. Versions are tracked through semantic version numbers in the filename (for example, `refactor-extract-function-v1.0.md` becomes `refactor-extract-function-v1.1.md` for minor improvements or `v2.0.md` for major changes). Each prompt file includes a header block with the version number, last updated date, author, and change notes. Git history provides the complete evolution, while the explicit version allows teams to reference specific prompt iterations (for example, "use the v1.2 code review prompt") and roll back if newer versions introduce problems. This provides a clear history of how prompting practices have improved over time.

**Performance considerations**: Prompts should balance detail with efficiency. Extremely long prompts can increase response latency and API costs without proportional improvements in output quality. Developers are encouraged to test prompts in scratch branches first, compare response times and accuracy, and refine prompts to include only the context necessary for the task at hand.

### 2.8 Error Handling and Recovery
Errors are handled deliberately. When a model returns an ambiguous answer, the developer asks it to restate its understanding or to highlight missing information before proceeding. If Claude or Codex hallucinates changes or edits the wrong files, the prompt instructs the model to explain its reasoning so the human can identify the misunderstanding. Minor mistakes can be retried with a refined prompt; repeated failures signal that it is time to escalate to another tool or a human review.

Self-correction prompts are part of the toolkit. Developers ask the model to compare its proposal against the documented requirements, list unsupported assumptions, or show how the change will be verified. If those responses remain vague, the workflow pauses and the developer either rewrites the prompt from scratch or hands the problem to a teammate. Treating ambiguous output as a warning rather than a shortcut keeps the workflow safe.

**Security note**: Prompts and their archived outputs must never contain credentials, API keys, or personally identifiable information. Developers should sanitize logs, test output, and error messages before including them in prompts that will be stored in issue comments or public repositories. If sensitive data appears in a prompt by mistake, the issue or pull request comment should be edited or deleted immediately, and the incident should be noted in the security log.

### 2.9 Integration with Playbooks
This section describes the philosophy and flow; the actionable templates will be developed in the Prompting Playbook. The playbook will contain reusable context blocks, safety checklists, and model-specific templates referenced throughout Section 2. As new prompt patterns emerge from active projects, they will be added to the playbook and linked from the relevant lifecycle stage (planning, implementation, testing, or review). The combination of this narrative and the playbook keeps prompting both consistent and easy to evolve.

## 3. Planning and Requirements Workflow

For detailed guidance, see [Planning and Requirements Workflow](workflows/planning-and-requirements.md).

### Starting from an idea
Explain how raw ideas enter the workflow, who captures them, and where they are tracked.

### Using AI for structured planning
Highlight the moments where Claude Code or Codex helps transform ideas into scoped work.

### Creating or updating requirements
Describe how requirements documents or backlog entries are drafted or revised before work begins.

### Turning plans into actionable work
Show how plans become issues, tasks, or branches with clear owners and outcomes.

### When planning is considered complete
State the exit criteria for planning so teams know when to move into design.

## 4. Design and Architecture Workflow

For detailed guidance, see [Design and Architecture Workflow](workflows/design-and-architecture.md).

### When to create or update design artifacts
Clarify the triggers for writing or refreshing design documents.

### Using AI tools for design
Share how Claude Code or Codex supports evaluating tradeoffs or producing diagrams and design notes.

### Design documents and where they live
Point to the directories or templates that store design artifacts.

### Validating design before implementation
Describe the checks, peer reviews, or spikes required before coding starts.

### Criteria for design readiness
List the signals that the design is stable enough for implementation.

## 5. Implementation Workflow

For detailed guidance, see [Implementation and Testing Workflow](workflows/implementation-and-testing.md).

### Branch creation and task setup
Detail how branches map to tasks and how gh or git commands are used to prepare the workspace.

### Coding style: test driven or code first
Explain how developers decide between TDD, targeted tests, or exploratory coding for each change.

### Role of Copilot during implementation
Summarize how Copilot accelerates day-to-day development without replacing code review.

### Role of Claude Code during implementation
Clarify when Claude Code performs repo-aware edits, refactors, or structured tasks mid-stream.

### Mid-implementation checkpoints with Codex
Describe how Codex offers periodic audits, validations, or alternate solutions while work is in progress.

### When implementation is considered complete for a branch
State the completion criteria before the branch moves to testing and review.

## 6. Testing Workflow

### Local unit testing
Explain expectations for unit coverage, tooling, and where test commands live.

### Integration and functional testing
Outline how broader tests are run locally or in lightweight environments.

### Using AI to write and improve tests
Share how Copilot, Claude Code, and Codex assist with test authoring, refactoring, and analysis.

### Recording test results
Document how test outcomes are captured for reviewers, including logs or summaries.

### Future continuous integration testing
Note the plans for CI adoption and how future automation will hook into this workflow.

## 7. Peer Review and AI-Assisted Review

For detailed guidance, see [Review and Merge Workflow](workflows/review-and-merge.md).

### Creating a pull request
Describe the gh commands, templates, and metadata expected when opening a PR.

### Claude Code-based review
Explain how Claude Code reviews diffs, suggests refinements, and raises questions.

### Codex-based independent review
Show how Codex provides a second perspective or risk assessment during review.

### Human review and follow-up changes
Clarify how human reviewers engage with AI findings and when manual edits are required.

### When review is considered complete
State the combination of approvals, checks, and responses needed before merging.

## 8. Documentation Update Workflow

For detailed guidance, see [Documentation Updates Workflow](workflows/documentation-updates.md).

### Deciding what documentation to update
List the cues used to determine when a change requires doc updates.

### Using Claude to draft documentation
Explain how Claude Code helps produce or revise structured documentation content.

### Using Codex to review documentation
Describe how Codex reviews readability, consistency, and coverage.

### Keeping documentation aligned with code
Note how doc changes are tested or cross-checked against code updates.

### Completion criteria for documentation
Provide the definition of done for doc updates before merge.

## 9. Merge and Post-Merge Validation

For detailed guidance, see [Merge and Validation Workflow](workflows/merge-and-validation.md).

### Pre-merge checklist
Summarize the checklist items verified before pressing merge.

### Merging the pull request
Detail the commands or GitHub UI steps for merging once approvals are in place.

### Post-merge sanity checks
Explain the quick validations run immediately after merge to confirm stability.

### Functional and higher-level testing
Describe how larger tests, demos, or manual validation round out the process.

## 10. Continuous Improvement of Workflows

For detailed guidance, see [Continuous Improvement Workflow](workflows/continuous-improvement.md).

### Capturing lessons learned
Show how retro notes, incident reviews, or daily learnings are collected.

### Refining prompts and templates
Describe how prompt libraries, templates, or guides evolve based on usage.

### Evolving the toolchain
Explain the process for adding, upgrading, or retiring tools in the stack.

### Periodic review of this document
State the cadence for reviewing and updating the workflows reference and linked playbooks.
