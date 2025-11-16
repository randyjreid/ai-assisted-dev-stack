# Workflows

This document describes the complete lifecycle for the AI Assisted Dev Stack, from the first idea to the moment a change is merged and validated. It shows how prompting, planning, design, implementation, testing, review, documentation, and continuous improvement fit together, with explicit touchpoints for Claude Code, Copilot, Codex, and the rest of the toolchain. Detailed, step by step instructions for each stage will be captured in the playbooks under `docs/workflows/`.

## 1. Workflow Overview

### 1.1 Purpose of These Workflows
The workflow system exists to make every change predictable, auditable, and aligned with the stack conventions while still leaving space for experimentation. Predictability comes from defining explicit entry and exit criteria for each stage so work follows repeatable patterns and the team knows what “ready” looks like before moving forward. Auditability is achieved by keeping a clear chain of evidence: prompts point to issues, issues point to branches, branches point to pull requests, and decisions are captured in review threads, documentation, and workflow logs. Alignment with stack conventions means branching strategies tie back to roadmap items, pull request templates enforce review expectations, and documentation checkpoints cover the overview, components, install, and workflow guides whenever a change modifies behavior or process.

These workflows also provide controlled spaces for experimentation. Dedicated branches or sandboxes allow new prompt strategies, tool combinations, or automation scripts to be tested without risking the main branch. Successful experiments feed back into the standard workflow so good ideas become part of the baseline instead of one off wins. Each workflow stage is explicitly tied to the roadmap phases: Phase 0 established the scaffolding and baseline documentation, Phase 1 focuses on codifying the core stack documentation and workflow definitions, and later phases will introduce automation scripts, deeper workflow playbooks, public documentation, and an evolving toolchain.

### 1.2 High Level Lifecycle
The lifecycle begins with prompting and intent, where the goal is to capture the problem statement and the context required by AI tools. Ideas, issues, and user feedback act as the primary inputs, and the outputs are refined goals, initial prompts, and shared context that align with roadmap priorities. Planning and requirements then translate that intent into actionable work by producing task breakdowns, requirement updates, and acceptance criteria. This stage consumes prompting outcomes and existing documentation, and it can loop with the prompting stage when clarification is needed.

Design and architecture follows, defining the technical approach, interfaces, and constraints. It relies on the plans and requirements and produces design notes, diagrams, and decision records that guide the next stages. Implementation uses those artifacts to build the change in a branch through incremental commits, generating code updates, local tests, and status reports. Testing validates that work by running unit, integration, and functional checks, turning code changes and test plans into test results, bug reports, and readiness signals. Testing may run in parallel with final implementation touches when quick fixes are needed.

Review ensures the change meets quality expectations through a mix of AI assisted and human analysis. Pull requests, diffs, test evidence, and documentation updates feed into this stage, which produces approvals, review notes, and follow up tasks. Documentation updates ensure the overview, components, install, workflows, and playbooks stay synchronized with the code. Once documentation aligns with the latest implementation and review feedback, merge and post merge validation integrate the change into the main branch, creating merged commits, tags, and validation records. Immediate sanity checks happen here, and any issues can trigger new planning or bug work.

The cycle closes with continuous improvement. Retro notes, validation outcomes, and roadmap signals are reviewed to refine prompts, adjust workflows, and plan tool upgrades. Insights captured in this phase flow directly into the next prompting and planning effort, so the lifecycle is both sequential and iterative. Each stage depends on the previous stage’s outputs, yet feedback loops allow planning, design, implementation, and documentation to run in parallel when a change requires rapid iteration. These handoffs align with the roadmap checkpoints so each phase delivers a measurable outcome before the next begins.

### 1.3 Role of Tools Across the Lifecycle
Git and GitHub serve as the system of record across every stage. They host branches, commits, pull requests, issues, and roadmap entries, and they provide the links that tie documentation, prompts, and design notes back to a single history. GitHub CLI builds on that foundation by handling branch creation, status checks, pull request operations, and reviews directly from the terminal. It exposes the same data as the GitHub web interface, which makes it easy to share context with AI tools through commands and outputs, while leaving the IDE free for editing and testing.

Visual Studio Code is the integration point for design notes, implementation work, testing, and documentation updates. It houses the local files, WSL terminals, and extensions that connect Copilot, Claude, and Codex into a single workspace, but it is not the authoritative history. GitHub Copilot focuses on the inner loop during implementation, testing, and lightweight documentation edits by offering inline suggestions based on the files that are currently open. It is best suited for small changes, patterns, and quick scaffolding rather than repo wide transformations.

Claude Code CLI provides structured, repository aware assistance for planning refinement, design exploration, implementation tasks, documentation updates, and review support. It consumes the full repository context and prompt history, making it ideal for coordinated edits that touch multiple files, though every change still goes through human review before merge. OpenAI Codex CLI offers a complementary perspective during planning, validation, review, and continuous improvement. It is commonly used for second opinions, alternative solutions, or root cause analysis, and it works best when prompts clearly describe the desired outcome and reference existing documentation.

### 1.4 Inputs and Outputs of the Workflow System
The workflow system consumes a steady stream of inputs: ideas from users or stakeholders, roadmap entries, bug reports, requirements, existing documentation, design artifacts, tool diagnostics, retrospective notes, and prompt libraries. Each input feeds into the lifecycle stage where it adds the most value. The outputs are merged code and tests that satisfy the requirements, updated documentation across all guides and playbooks, roadmap adjustments, version history entries, and refined workflow practices.

Section 2 onward dives deeper into each lifecycle stage and links to the related playbooks that explain the day to day execution. The inputs and outputs defined here describe how information flows between stages, tools, and documents, ensuring the later sections have a clear set of boundaries and expectations to build on.

## 2. Prompting and Interaction Patterns

For detailed guidance, see [Prompting Playbook](workflows/prompting-playbook.md).

### Principles of prompt use in this stack
List the values that drive good prompting such as clarity, transparency, and traceability of AI sourced output.

### Shared context for AI tools
Describe how repositories, docs, and prior prompts supply context that carries across sessions and tools.

### Prompt patterns for Claude Code
Outline the structured prompt templates used when Claude Code performs repository wide tasks.

### Prompt patterns for Codex
Call out the prompts used to request independent reviews, explorations, or automation tasks from Codex.

### How Copilot fits alongside prompt based tools
Note how inline Copilot suggestions complement longer prompt driven exchanges without duplicating effort.

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
Summarize how Copilot accelerates day to day development without replacing code review.

### Role of Claude Code during implementation
Clarify when Claude Code performs repo aware edits, refactors, or structured tasks mid stream.

### Mid implementation checkpoints with Codex
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

## 7. Peer Review and AI Assisted Review

For detailed guidance, see [Review and Merge Workflow](workflows/review-and-merge.md).

### Creating a pull request
Describe the gh commands, templates, and metadata expected when opening a PR.

### Claude Code based review
Explain how Claude Code reviews diffs, suggests refinements, and raises questions.

### Codex based independent review
Show how Codex provides a second perspective or risk assessment during review.

### Human review and follow up changes
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
Note how doc changes are tested or cross checked against code updates.

### Completion criteria for documentation
Provide the definition of done for doc updates before merge.

## 9. Merge and Post Merge Validation

For detailed guidance, see [Merge and Validation Workflow](workflows/merge-and-validation.md).

### Pre merge checklist
Summarize the checklist items verified before pressing merge.

### Merging the pull request
Detail the commands or GitHub UI steps for merging once approvals are in place.

### Post merge sanity checks
Explain the quick validations run immediately after merge to confirm stability.

### Functional and higher level testing
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
