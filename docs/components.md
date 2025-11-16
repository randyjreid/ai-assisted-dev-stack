# Components

## Overview

This document describes each component that forms the AI Assisted Dev Stack and explains the role it plays within the overall workflow. For context on the high level architecture and how these components fit together, see [`overview.md`](overview.md).

 Every tool was selected because it contributes directly to a stable and efficient development environment. Some components provide the operating foundation, others enhance productivity through intelligent automation, and several deliver the AI assisted capabilities that define this stack. Understanding why each tool is included and how it fits into the broader system ensures that the environment remains easy to rebuild, easy to extend, and grounded in real world use.

## WSL Ubuntu 24.04

WSL Ubuntu 24.04 serves as the base layer of the environment. It provides a full Linux system running inside Windows without the overhead of a traditional virtual machine. This gives me a predictable, consistent development surface while still allowing me to use Windows applications and hardware. Because the stack depends heavily on Linux based tooling, WSL ensures that package installation, command line tools, and AI utilities all behave exactly as intended.

Ubuntu 24.04 is a long term support release, which makes it well suited for a reproducible environment that I may rebuild on future machines. It receives extended security updates and maintains a stable package ecosystem. WSL integrates tightly with Visual Studio Code, allowing the editor to run on Windows while all code, commands, scripts, and AI tooling operate inside Linux. This separation keeps the workflow consistent and avoids the subtle issues that often occur when mixing Windows and Linux tooling. WSL ultimately provides the reliability and flexibility needed for the rest of the stack to function smoothly.

## Visual Studio Code

Visual Studio Code is the primary editing environment and the central place where the Linux system, AI tools, and GitHub workflows come together. When I open a project through the Remote WSL extension, the editor connects directly to the Ubuntu environment while still running as a Windows application. This approach gives me full access to Linux tooling while retaining the convenience and usability of Visual Studio Code.

The extension ecosystem significantly enhances productivity. GitHub Copilot integrates directly into the editor to provide inline suggestions that accelerate everyday development tasks. GitLens improves repository awareness and navigation. The built in terminal runs inside WSL and keeps all commands aligned with the Linux environment. Together, these capabilities make Visual Studio Code the natural hub for writing, testing, refactoring, reviewing, and managing code in the AI Assisted Dev Stack.

## Git and GitHub

Git and GitHub form the foundation of version control and collaboration across the entire environment. Git provides a reliable system for tracking changes, branching, merging, and maintaining a complete history of the project. Treating each change as a pull request, even when working independently, adds structure to the development process and creates a clear record of decisions and revisions.

GitHub builds upon this by hosting repositories and offering tools such as pull requests, reviews, protected branches, and security insights. Because the stack is designed to be reproducible and easy to move between machines, GitHub serves as the long term home for both code and documentation. Maintaining everything on GitHub ensures that every project benefits from a consistent workflow and a single source of truth.

## GitHub CLI (gh)

GitHub CLI extends the capabilities of GitHub into the command line. It enables authentication, pull request creation, branch management, issue handling, and repository exploration without leaving the terminal. This reduces context switching and keeps the workflow focused on the task at hand.

Using GitHub CLI makes pull request driven development more efficient. After completing or generating changes, I can review modifications, commit them, and open a pull request directly from inside WSL. This works especially well with Claude Code and Codex, since their output can be incorporated immediately into the repository. GitHub CLI also supports extensions and scripting, which will become increasingly valuable in later phases as more automation is introduced.

## GitHub Copilot

GitHub Copilot is the first AI tool in the stack and plays a central role in daily development. It operates inside Visual Studio Code and provides inline suggestions that speed up coding, reduce repetitive work, and help explore alternative approaches. Copilot focuses on micro level interactions and responds instantly to the current context in the editor.

Because Copilot works as I type, it helps maintain momentum during feature development, refactoring, and experimentation. It is especially useful for writing functions, generating patterns, and making small adjustments. Copilot complements the other AI tools by handling the immediate, hands on portions of development that do not require deep structural analysis. It improves productivity without interrupting the natural flow of coding.

## Claude Code CLI

Claude Code CLI is the primary large language model tool for structured development tasks that span multiple files. It is able to understand the entire repository, analyze multiple files simultaneously, and produce coordinated changes that maintain consistency across the project. This makes it ideal for feature implementations, large refactors, documentation updates, and other changes that benefit from broader context.

Claude Code is especially effective when global reasoning is required. It can read documentation, identify patterns in the codebase, and generate solutions that match established conventions. Because it operates at a higher level than Copilot, it often handles the tasks that require deliberate planning and structural understanding. After generating changes, I can review the output and use GitHub CLI to create a pull request for further analysis.

## OpenAI Codex CLI

The OpenAI Codex CLI acts as an independent reviewer and analysis partner. It provides a different perspective from both Copilot and Claude Code, which makes it valuable when evaluating pull requests or checking architectural decisions. Codex helps identify potential issues, suggests improvements, and highlights edge cases or questions that may not be immediately obvious.

Codex is particularly helpful during the review phase. After Claude Code creates an implementation, I can use Codex to examine the changes, confirm the design approach, and strengthen confidence before merging the pull request. By combining Codex with GitHub CLI, the entire review process stays inside the WSL environment and remains consistent with the rest of the workflow.

## Supporting CLI Tools

Several supporting command line tools enable installation, scripting, automation, and general development tasks throughout the stack. curl and wget provide reliable ways to retrieve files and installation scripts. jq allows easy parsing and manipulation of JSON output from GitHub CLI and other tools. fzf improves navigation by enabling quick fuzzy searching across files, history, and commands. unzip ensures smooth extraction of compressed archives. The build essential package provides the compilation tools needed to install or compile dependencies that rely on native extensions.

While each of these utilities is simple on its own, together they provide the stability, flexibility, and convenience needed for a productive development environment. They ensure that the underlying Linux system can support more advanced workflows without interruption.

## Summary

The components described in this document work together to create a cohesive, reliable, and extensible development environment. Each tool serves a distinct purpose, and the combination of Linux based tooling, GitHub centric workflows, Visual Studio Code, and AI models forms a powerful foundation for modern software development. This document will continue to evolve as the stack grows and as new tools become valuable in future phases.

For step by step installation instructions for these components, see [`install.md`](install.md).
