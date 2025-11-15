# AI-Assisted Dev Stack

A complete, evolving AI-assisted development environment for modern software engineering.  
This repository documents a reproducible setup for building software using a multi-LLM workflow that integrates Claude Code CLI, OpenAI/Codex CLI, GitHub Copilot, GitHub CLI, WSL Ubuntu, and Visual Studio Code.

The goal is to maintain a stable, well-documented baseline that I can rebuild anytime, extend as new tools arrive, and share publicly to help others adopt AI-augmented development workflows.

A dedicated [Workflows guide](docs/workflows.md) now explains how ideas move from initial prompts through merged changes and how each AI tool supports that flow.

---

## ⚡️ Why This Exists

AI-assisted development is becoming the standard approach to writing software.  
Claude, Copilot, Codex, GitHub CLI, and other tools dramatically improve productivity, but only when they are combined into a coherent, repeatable environment.

This repository serves as:

- A reproducible framework for rebuilding my development environment as needed.  
- A living reference documenting tools, workflows, and updates.  
- A public blueprint for anyone interested in modern AI-assisted engineering.  

---

## 🧩 Key Features

- WSL 2 Ubuntu 24.04 development environment  
- Visual Studio Code with curated extensions and WSL integration  
- GitHub CLI (`gh`) for streamlined GitHub workflows  
- Claude Code CLI for feature implementation and repo-wide reasoning  
- OpenAI/Codex CLI for review, validation, and automation  
- GitHub Copilot for inline coding, autocompletion, and refactors  
- Scriptable setup (future phases)  
- Documented workflows for feature development, PR reviews, and testing  
- Optional tools planned: Aider, Continue.dev, Docker, Ollama  
- Organized docs, templates, and examples  

---

## 📁 Repository Structure

```
ai-assisted-dev-stack/
├── docs/               # Detailed documentation (installation, components, workflows)
│   ├── overview.md
│   ├── install.md
│   ├── workflows.md
│   ├── components.md
│   ├── update-log.md
│   └── roadmap.md
│
├── scripts/            # Setup, verification, and automation scripts (future phases)
│   ├── bootstrap_wsl.sh
│   ├── install_core_tools.sh
│   ├── install_ai_tools.sh
│   └── verify_stack.sh
│
├── .vscode/            # Recommended VS Code extensions and settings
│   ├── extensions.json
│   └── settings.json
│
├── examples/           # Templates, prompts, aliases, and workflow samples
│   ├── prompt-templates.md
│   ├── shell-aliases.md
│   └── git-workflows.md
│
└── README.md           # You are here
```

All files begin as placeholders and will be expanded in upcoming phases.

---

## 🚀 Getting Started

This repository is currently in **Phase 0**: scaffolding and documentation structure.

### Prerequisites
- Windows 10/11 with WSL 2  
- Ubuntu 24.04 installed via WSL  
- Git  
- Visual Studio Code (with WSL extension)  

### Clone the Repository

```bash
cd ~
mkdir -p code
cd code
git clone https://github.com/randyjreid/ai-assisted-dev-stack.git
cd ai-assisted-dev-stack
```

Documentation for installation and usage lives in:

- [x] `docs/overview.md`  
- [x] `docs/install.md`  
- [x] `docs/workflows.md`

---

## 🗂 Documentation Index

The following documents will be expanded as the stack evolves:

- [x] **overview.md** – Purpose, philosophy, system architecture  
- [x] **install.md** – Step-by-step installation instructions  
- [x] **components.md** – Detailed breakdown of each tool  
- [x] **workflows.md** – Lifecycle guidance from idea to merged change with links to detailed playbooks  
- [ ] **update-log.md** – Version history of the stack  
- [ ] **roadmap.md** – Planned enhancements  

Detailed workflow playbooks will live under `docs/workflows/` as each stage is expanded.

---

## 🛠 Roadmap

### Phase 0 – Repo Scaffolding (Current)
- [x] Create initial directory and documentation structure  
- [x] Add README and base files  
- [x] Add Dependabot configuration  

### Phase 1 – Core Stack Documentation
- [ ] WSL setup  
- [ ] VS Code setup  
- [ ] GitHub CLI  
- [ ] Claude Code CLI  
- [ ] OpenAI CLI (Codex)  
- [ ] GitHub Copilot  

### Phase 2 – Setup Scripts
- [ ] WSL bootstrap script  
- [ ] Core tool installation scripts  
- [ ] AI tool installation automation  
- [ ] Stack verification script  

### Phase 3 – Workflow Documentation
- [ ] Branching strategy  
- [ ] AI-assisted feature implementation  
- [ ] AI-driven PR review workflows  
- [ ] Testing and validation  

### Phase 4 – Public Write-Ups
- [ ] Publish LinkedIn article  
- [ ] Publish Medium technical deep-dive  
- [ ] Invite community feedback  

### Phase 5 – Evolution
- [ ] Local LLM support (Ollama, LM Studio)  
- [ ] Agent workflows  
- [ ] Self-hosted GitHub Actions runners  
- [ ] Devcontainer-based environments  

---

## 🤝 Contributing

Suggestions, improvements, and ideas are welcome as AI-assisted development continues to evolve.  
Issues and pull requests can be opened at any time.

---

## 📄 License

This project is licensed under the **MIT License**.
