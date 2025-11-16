# Prompt Library

This directory contains reusable prompt templates for AI-assisted development workflows using Claude Code CLI, OpenAI Codex CLI, and GitHub Copilot.

## Purpose

The prompt library operationalizes the principles defined in [`docs/workflows/prompting-playbook.md`](../workflows/prompting-playbook.md). Each template provides:
- Structured format for clear, reproducible prompts
- Context requirements and token budget guidance
- Worked examples demonstrating usage
- Version history for tracking evolution

## Directory Structure

```
docs/prompts/
├── context/           # Context assembly utilities
├── error-handling/    # Error recovery and debugging
├── examples/          # Complete worked examples
├── implementation/    # Code change templates
├── review/            # Code review and validation
└── templates/         # Meta-templates (prompt logs, new prompts)
```

## Templates by Category

### Context Templates
Utilities for assembling and managing context before prompting.

| Template | Purpose | Tool | Tokens |
|----------|---------|------|--------|
| [build-context-block](context/build-context-block-v1.0.md) | Universal context structure for multi-tool prompts | Any | ~5k |
| [prepare-large-file-context](context/prepare-large-file-context-v1.0.md) | Summarize files exceeding token limits | Any | ~3k |
| [summarize-commits](context/summarize-commits-v1.0.md) | Capture recent commit history for context | Any | ~2k |

### Implementation Templates
Templates for code changes, refactors, and bug fixes.

| Template | Purpose | Tool | Complexity |
|----------|---------|------|------------|
| [add-feature-boilerplate](implementation/add-feature-boilerplate-v1.0.md) | Scaffold new features with acceptance criteria | Claude Code | Medium |
| [refactor-extract-function](implementation/refactor-extract-function-v1.0.md) | Extract logic into dedicated helpers | Claude Code | Medium |
| [fix-bug-analysis](implementation/fix-bug-analysis-v1.0.md) | Analyze bugs before fixing | Claude Code | Low |

### Review Templates
Templates for code review, test analysis, and validation.

| Template | Purpose | Tool | Tokens |
|----------|---------|------|--------|
| [code-review-claude](review/code-review-claude-v1.0.md) | Structured review with Claude | Claude Code | ~70k |
| [code-review-codex](review/code-review-codex-v1.0.md) | Independent review with Codex | Codex | ~12k |
| [test-failure-analysis](review/test-failure-analysis-v1.0.md) | Diagnose failing tests | Claude/Codex | ~70k |

### Error Handling Templates
Templates for recovering from prompt failures and unexpected behavior.

| Template | Purpose | Tool | Tokens |
|----------|---------|------|--------|
| [partial-failure-followup](error-handling/partial-failure-followup-v1.0.md) | Address incomplete implementations | Claude Code | ~40k |
| [diagnose-drift-or-hallucination](error-handling/diagnose-drift-or-hallucination-v1.0.md) | Debug unexpected AI references | Claude/Codex | ~25k |
| [request-self-correction](error-handling/request-self-correction-v1.0.md) | Validate AI output before proceeding | Claude/Codex | ~20k |

### Examples
Complete worked examples demonstrating cross-tool workflows.

| Template | Purpose | Tools | Tokens |
|----------|---------|-------|--------|
| [copilot-to-claude-context](examples/copilot-to-claude-context-v1.0.md) | Handoff from Copilot to Claude | Copilot → Claude | 5k+60k |
| [claude-refactor-example](examples/claude-refactor-example-v1.0.md) | Complete refactor workflow | Claude Code | ~50k |
| [codex-review-example](examples/codex-review-example-v1.0.md) | Independent code review | Codex | ~8k |

### Templates (Meta)
Templates for creating new prompts and logging prompt interactions.

| Template | Purpose |
|----------|---------|
| [prompt-log-entry-template](templates/prompt-log-entry-template.md) | Standard format for archiving prompts |

## How to Use This Library

### 1. Choose the Right Template

**For context assembly**:
- Standard context block → `build-context-block`
- Large files → `prepare-large-file-context`
- Commit history → `summarize-commits`

**For implementation**:
- New feature → `add-feature-boilerplate`
- Refactor existing code → `refactor-extract-function`
- Bug fix → `fix-bug-analysis` (analysis), then implementation template (fix)

**For testing**:
- Test failures → `test-failure-analysis`

**For review**:
- Primary review → `code-review-claude`
- Second opinion → `code-review-codex`
- Test analysis → `test-failure-analysis`

**For error recovery**:
- Partial completion → `partial-failure-followup`
- Unexpected behavior → `diagnose-drift-or-hallucination`
- Validation check → `request-self-correction`

### 2. Customize the Template

1. Read the template's "When to Use / When Not to Use" section
2. Copy the template markdown from the `## Template` section
3. Replace `<placeholders>` with your specific details
4. Review the worked example for guidance
5. Adjust constraints and verification steps for your context

### 3. Execute the Prompt

1. Assemble context using context templates if needed
2. Send the customized prompt to the recommended tool (see frontmatter)
3. Validate the output using the verification steps in the template
4. Log the prompt and outcome using `templates/prompt-log-entry-template.md`

### 4. Refine and Iterate

- If the result is partial → use `partial-failure-followup`
- If the result is unexpected → use `diagnose-drift-or-hallucination`
- If you need validation → use `request-self-correction`
- Consult related templates listed in each template's frontmatter

## Tool Selection Guide

### Use Claude Code CLI when:
- Making coordinated multi-file changes
- Performing complex refactors across modules
- Generating documentation updates touching multiple directories
- You need repository-aware context and structured edits
- Tasks require large context windows (up to 200k tokens)

### Use OpenAI Codex CLI when:
- Requesting independent code review
- Seeking alternative design perspectives
- Analyzing risks, root causes, or failure modes
- You want a second opinion or critical analysis (not implementation)
- Tasks benefit from a fresh perspective without prior context

### Use GitHub Copilot when:
- Writing inline suggestions and scaffolding
- Generating boilerplate or repetitive code patterns
- Quick refactors within a single file
- You're actively editing and want real-time assistance
- Speed matters more than multi-file coordination

### Cross-Tool Workflows

**Recommended pattern**: Copilot → Claude Code → Codex

1. **Copilot**: Generate initial scaffolding and boilerplate
   - Capture snippets with comments noting AI origin
   - Commit early with clear attribution

2. **Claude Code**: Coordinate multi-file implementation
   - Reference Copilot work in context block
   - Use implementation templates for structured changes
   - Validate with tests and linting

3. **Codex**: Independent review and validation
   - Request second opinion on approach
   - Identify risks and edge cases
   - Suggest additional tests or safeguards

See `examples/copilot-to-claude-context-v1.0.md` for a complete handoff workflow.

## Contributing

### Adding a New Template

1. **Naming convention**: `{action}-{object}-v1.0.md`
   - Examples: `refactor-extract-function-v1.0.md`, `code-review-claude-v1.0.md`

2. **Required frontmatter**:
   ```yaml
   ---
   title: Template Title
   purpose: Brief description of what this template does
   recommended_tool: Claude Code CLI / OpenAI Codex CLI / Any
   context_requirements:
     - Requirement 1
     - Requirement 2
   token_budget_notes: Guidance on token usage
   related_prompts:
     - related-template-1-v1.0.md
     - related-template-2-v1.0.md
   version_history:
     - v1.0 (YYYY-MM-DD): Initial template
   ---
   ```

3. **Required sections**:
   - `## Template` - The actual prompt template with placeholders
   - `## When to Use` - Scenarios where this template is appropriate
   - `## When Not to Use` - Scenarios to avoid
   - `## Worked Example` - Complete, realistic example
   - `## Versioning Notes` - How to increment versions

4. **Update this README**:
   - Add entry to appropriate category table
   - Include template name, purpose, tool, and complexity/tokens
   - Link to the new template file

### Updating an Existing Template

1. **Version increment rules**:
   - **Minor (v1.0 → v1.1)**: Clarifications, improved examples, small wording changes
   - **Major (v1.x → v2.0)**: Structural changes, new required fields, changed workflow

2. **Update process**:
   - Increment version in filename and frontmatter
   - Update `version_history` block with date and reason
   - Document changes in "Versioning Notes" section
   - Consider keeping old version temporarily if major change

3. **Cross-reference updates**:
   - Update `related_prompts` in affected templates
   - Update this README if purpose or tool changes
   - Notify team if breaking changes affect existing workflows

### Deprecating Templates

When retiring a template:
1. Add `deprecated: true` to frontmatter
2. Add `superseded_by: new-template-v2.0.md` if applicable
3. Keep file in place for 2-3 months with deprecation notice
4. Update this README to mark as deprecated
5. Remove from this README after grace period

## Related Documentation

- [Prompting Playbook](../workflows/prompting-playbook.md) - Detailed guidance on prompting discipline
- [Workflows Overview](../workflows.md) - Complete workflow lifecycle
- [Workflows Section 2](../workflows.md#2-prompting-and-interaction-patterns) - Prompting philosophy and patterns

## Version

**Library Version**: 1.0
**Last Updated**: 2025-02-15

For questions or suggestions, consult the [Prompting Playbook](../workflows/prompting-playbook.md) or raise an issue in the project repository.
