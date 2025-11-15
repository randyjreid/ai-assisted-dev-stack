# Workflows

This document describes the complete lifecycle for the AI Assisted Dev Stack, from the first idea to the moment a change is merged and validated. It shows how prompting, planning, design, implementation, testing, review, documentation, and continuous improvement fit together, with explicit touchpoints for Claude Code, Copilot, Codex, and the rest of the toolchain. Detailed, step by step instructions for each stage will be captured in the playbooks under `docs/workflows/`.

## 1. Workflow Overview

### Purpose of these workflows
Explain how the workflow set keeps work predictable, auditable, and aligned with stack conventions while leaving room for experimentation.

### High level lifecycle
Describe the sequence that links prompting, planning, design, implementation, testing, review, documentation, and merge activities.

### Role of tools across the lifecycle
Summarize when Copilot, Claude Code, Codex, GitHub CLI, and other components are most helpful and how they share context across stages.

## 2. Prompting and Interaction Patterns

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

### Pre merge checklist
Summarize the checklist items verified before pressing merge.

### Merging the pull request
Detail the commands or GitHub UI steps for merging once approvals are in place.

### Post merge sanity checks
Explain the quick validations run immediately after merge to confirm stability.

### Functional and higher level testing
Describe how larger tests, demos, or manual validation round out the process.

## 10. Continuous Improvement of Workflows

### Capturing lessons learned
Show how retro notes, incident reviews, or daily learnings are collected.

### Refining prompts and templates
Describe how prompt libraries, templates, or guides evolve based on usage.

### Evolving the toolchain
Explain the process for adding, upgrading, or retiring tools in the stack.

### Periodic review of this document
State the cadence for reviewing and updating the workflows reference and linked playbooks.
