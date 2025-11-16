# Roadmap

This document tracks the evolution of the AI Assisted Dev Stack, including completed phases, current work, and future enhancements planned for later releases.

## Phase 0 – Repository Scaffolding (Complete)

- [x] Created initial directory and documentation structure
- [x] Added README and base files
- [x] Configured Dependabot for dependency management

## Phase 1 – Core Stack Documentation (In Progress)

- [x] Documented WSL Ubuntu 24.04 setup
- [x] Documented Visual Studio Code setup and integration
- [x] Documented GitHub CLI installation and authentication
- [x] Documented Claude Code CLI installation and workflows
- [x] Documented OpenAI Codex CLI installation and usage
- [x] Documented GitHub Copilot integration
- [x] Created workflow overview and playbooks structure
- [ ] Complete detailed playbook content
- [ ] Finalize examples and templates

## Phase 2 – Setup Scripts (Planned)

- [ ] WSL bootstrap script
- [ ] Core tool installation automation
- [ ] AI tool installation automation
- [ ] Stack verification and health check script

## Phase 3 – Workflow Documentation (Planned)

- [ ] Detailed branching strategy
- [ ] Feature implementation patterns
- [ ] Pull request review workflows
- [ ] Testing and validation procedures

## Phase 4 – Public Write-Ups (Planned)

- [ ] LinkedIn article on the stack
- [ ] Medium technical deep dive
- [ ] Community feedback collection

## Phase 5 – Evolution (Planned)

- [ ] Local LLM support (Ollama, LM Studio)
- [ ] Agent workflow patterns
- [ ] Self hosted GitHub Actions runners
- [ ] Devcontainer based environments

---

## Optional and Future Components

This section describes additional tools that may be incorporated into later phases of the AI Assisted Dev Stack. These components are either planned for future integration or under consideration based on their potential value. Each tool is included here because it strengthens reproducibility, automation, or workflows that use multiple LLMs.

### Docker and Dev Containers (Planned)

Docker Containers provide fully reproducible development environments. A future phase may introduce a Dev Container that installs all required tools (Copilot, Claude, Codex, gh, Node, Python, etc.) so a new machine can be configured instantly. This will eventually become the recommended installation method once the stack stabilizes.

### Python Environment Managers (Planned)

Tools such as `pyenv`, `uv`, or `virtualenv` help avoid conflicts when working across multiple projects. Phase 2 or 3 may introduce a standard project template with:

- isolated virtual environments
- dependency snapshots
- standardized tooling (ruff, mypy, pytest)

This will support more advanced ML and AI development as the stack grows.

### Aider and Continue.dev (Under Consideration)

Aider and Continue.dev enable conversational coding in an agent style, directly from within the terminal or IDE. These tools complement Claude Code and Codex by adding:

- file diffs with fine detail
- conversational refactoring
- patch generation in clear steps

They may be added once the core workflow stabilizes and we determine whether they provide measurable value beyond the existing tools.

### GitHub Actions (Planned)

Future phases will introduce CI/CD automation for:

- linting
- testing
- documentation generation
- automated pull request checks
- security scanning

Adding Actions ensures the dev stack scales into production grade engineering workflows.

### Local LLM Runtimes (Under Consideration)

Tools such as **Ollama** and **LM Studio** allow running lightweight or open source models locally for:

- offline coding assistance
- fast prototype loops
- model experimentation

These will be considered in later phases once the primary cloud workflow is fully established.

### Automated Bootstrap Scripts (Planned)

Once the environment is proven stable and repeatable, a Phase 2 or Phase 3 goal is to add:

- a single command bootstrap script
- system health checks
- dependency validation
- repair utilities

This will allow rebuilding the entire dev environment on a new machine in minutes.

### DevPod (Under Consideration)

DevPod enables reproducible, cloud backed or local dev environments and competes with Dev Containers and GitHub Codespaces but works offline. It may become relevant for usage across multiple machines in later phases.

### Additional Tools Under Consideration

These tools are not currently in scope but may be evaluated for potential inclusion:

- **tmux / Zellij** for advanced terminal workflows
- **fzf plugins** for navigation, history, and git integration
- **Starship prompt** for developer ergonomics
- **Neovim** for users who prefer modal editing
- **Tree-sitter** for enhanced code navigation
- **OpenAI or Anthropic Assistants frameworks** for building utilities with agent capabilities
- **Model monitoring tools** such as Langfuse (if future ML projects require them)

Each of these will be added only if they improve reproducibility, maintainability, or productivity without introducing unnecessary complexity.
