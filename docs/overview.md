# Overview

The AI Assisted Dev Stack is a reproducible and fully documented development environment designed to integrate multiple LLM powered tools into a cohesive, modern engineering workflow. It captures everything I use, from WSL and Visual Studio Code to GitHub CLI, GitHub Copilot, Claude Code, and Codex, and distills the real world lessons learned as I refine how I work. This stack provides a stable foundation that I can recreate on any machine while remaining flexible enough to evolve as new tools and practices emerge.

## 1. Purpose of This Project

This project provides a single authoritative reference for my development environment. It documents installation steps, tool configurations, workflows, and architectural rationale so that I can rebuild the environment quickly and predictably. Over time, this repository also serves as a historical record of how my tools, practices, and decisions have matured.

The documentation created in Phase 1 will later support installation scripts, environment verification tooling, and deeper workflows introduced in later phases. Because the repository is public, it also offers a transparent look at how I structure and maintain a multi LLM development workflow.

## 2. Philosophy and Motivation

My approach to development with AI centers on using large language models to augment, not replace, my engineering judgment. In this context, using AI in development means leveraging LLMs to generate code, analyze designs, identify edge cases, review changes, and automate routine tasks, while I remain responsible for architectural intent and quality.

Everything in this stack is grounded in practical use. As I work with these tools daily, I refine the workflow, adopt what works, discard what does not, and update the documentation accordingly. This iterative process ensures the stack remains centered on real world effectiveness rather than theoretical best practices.

## 3. High Level Architecture

The environment uses a layered architecture that blends the strengths of Windows, Linux, and modern AI tooling:

```
Windows 10/11
└── WSL 2 (Ubuntu 24.04)
    ├── Core utilities (curl, jq, git, build-essential)
    ├── Git and GitHub CLI
    ├── AI tooling:
    │     ├── GitHub Copilot
    │     ├── Claude Code CLI
    │     └── OpenAI Codex CLI
    ├── Visual Studio Code (WSL integrated)
    └── Future automation and scripts
```

Windows provides the host environment and user interface. WSL delivers a clean and predictable Linux layer with direct access to native tooling. Visual Studio Code serves as the editing interface, connected to the Linux backend through its WSL extension. GitHub and `gh` support version control, collaboration, and automation. The AI tooling layer sits above these foundations, supporting an efficient multi agent workflow.

## 4. Scope of This Stack

This project focuses strictly on the core development environment: installation, tooling, configuration, and patterns for AI assisted development. It intentionally does **not** include, at this stage:

- CI/CD pipelines  
- Infrastructure provisioning  
- Devcontainer or Docker based environments  
- Automated agents or self hosted runners  

These may be added in later phases once the core is stable and fully documented.

## 5. Core Components of the Stack

**WSL Ubuntu 24.04**  
A reliable Linux environment with excellent compatibility for modern tooling and seamless integration with Windows.

**Visual Studio Code**  
My primary editor, running directly against the WSL backend. It provides a consistent development experience with rich extensions and AI integrations.

**Git and GitHub**  
The backbone of source control, collaboration, and version history. Pull request workflows and branching strategies revolve around GitHub.

**GitHub CLI (`gh`)**  
Extends GitHub capabilities into the terminal and enables consistent workflows for authentication, pull request management, and future automation.

**GitHub Copilot**  
Inline, context aware code completion that accelerates refactors, patterns, exploration, and iterative development inside Visual Studio Code.

**Claude Code CLI**  
My primary LLM development assistant for multi file reasoning, structured implementations, refactors, and repository level analysis.

**OpenAI Codex CLI**
A secondary reviewer that offers alternative reasoning, deeper analysis, and a different perspective on code quality and design.

**Supporting CLI Tools**  
Utilities such as `curl`, `wget`, `jq`, `fzf`, `unzip`, and the `build-essential` suite support scripting, installation, and daily development tasks.

Together, these components form a modular and extensible environment designed for AI augmented software engineering.

## 6. How the Tools Work Together

Each AI tool plays a distinct role in the workflow. GitHub Copilot operates inside the editor to accelerate small refinements and coding tasks through inline suggestions and pattern generation. Claude Code handles complex tasks that require global awareness of the repository, including feature implementations, refactoring across multiple files, and architectural analysis. Codex functions as an independent reviewer that provides critiques, alternative solutions, and additional insights.

GitHub CLI connects these workflows to GitHub, managing branches, pull requests, reviews, and repository interactions directly from the terminal. Visual Studio Code acts as the orchestration layer where all tools come together. This approach using multiple agents blends automation with human intent, ensuring I maintain full ownership of the code while benefiting from multiple AI perspectives.

For a detailed look at how these tools work together across the development lifecycle, see [`workflows.md`](workflows.md).

## 7. Design Principles

The stack is guided by five principles:

**Reproducible:**  
The environment must be simple to recreate on any machine using clear, documented steps.

**Transparent:**  
Every tool and workflow is documented so nothing depends on implicit assumptions.

**Modular:**  
Individual tools can be swapped out or upgraded without disrupting the overall workflow.

**Idempotent (future phases):**  
Setup scripts will be safe to run repeatedly without causing errors or inconsistent state.

**Experience driven:**  
Improvements come directly from practical, real world use rather than theory.

## 8. Optional and Future Tools

Several tools may be added in later phases as the stack evolves:

- Aider for iterative LLM coding
- Continue.dev for automation driven by agents within Visual Studio Code
- Docker or devcontainers for portable development environments
- Local LLM runtimes such as Ollama or LM Studio for offline development
- Workflows with AI agents and self hosted GitHub runners  

Each addition will be evaluated for stability, usefulness, and its ability to improve the overall workflow.

## 9. Roadmap Alignment

Phase 1 establishes the documentation foundation for the entire project. Later phases will introduce installation scripts, detailed workflows for AI assisted development, refined tooling, and eventually public facing documentation. The complete roadmap is available in `docs/roadmap.md`, and this overview serves as the anchor for everything that follows.

## 10. Reference Links

- **Installation Guide:** [`install.md`](install.md)  
- **Component Descriptions:** [`components.md`](components.md)  
- **Workflow Overview:** [`workflows.md`](workflows.md)  
- **Version History:** [`update-log.md`](update-log.md)  
- **Project Roadmap:** [`roadmap.md`](roadmap.md)  
