# Install

This document provides a structured, detailed installation plan for the AI Assisted Dev Stack. It is written as a technical outline intended to guide you through a clean and reproducible installation using Windows, WSL 2, Ubuntu 24.04, Visual Studio Code, GitHub services, and multiple AI assisted CLI tools.

Before starting installation, review [`overview.md`](overview.md) for the high level architecture and [`components.md`](components.md) for detailed descriptions of each tool.

---
## 1. Prerequisites

Before installation begins, ensure that all required accounts and credentials are available.

### 1.1 Required Accounts
- GitHub account for repositories, GitHub CLI, and GitHub Copilot.
- Microsoft or GitHub account if Visual Studio Code settings sync is desired.
- OpenAI account for the OpenAI Codex CLI.
- Anthropic account for Claude Code CLI access.

### 1.2 Secret Storage
Use ~/.bashrc for session-persistent environment variables.
Use /etc/environment for global variables (not recommended).
Do not use .profile or .bash_profile on Ubuntu (ignored in non-login shells).
Avoid committing secrets or `.env` files to Git repositories.
A more advanced secrets solution will be added in a future phase.

---
## 2. Install WSL and Ubuntu 24.04

### 2.1 Enable WSL
Run in elevated PowerShell:

```powershell
wsl --install -d Ubuntu-24.04
```

Reboot if required.

Check WSL distro version:
```powershell
wsl -l -v
```
Expect to see something like Ubuntu-24.04 Running 2

### 2.2 Initial Ubuntu Setup
Launch **Ubuntu 24.04** from Start Menu.
Create a Unix username and password.

Update base system:

```bash
sudo apt update
sudo apt upgrade -y
```

---
## 3. Install Core Linux Tooling (WSL Ubuntu)

### 3.1 Base Utilities
```bash
sudo apt install -y git curl wget jq fzf wslu unzip build-essential ca-certificates software-properties-common
```

### 3.2 Install Python and pip
```bash
sudo apt install -y python3 python3-pip python3-venv python3-dev
```

### 3.3 Install pipx
```bash
sudo apt install -y pipx
pipx ensurepath
```

pipx may recommend optional completions, which are not required.

---
### 3.4 Ensure ~/bin is on PATH
```bash
mkdir -p ~/bin
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 3.5 Verify Core Tools
```bash
git --version
curl --version
jq --version
fzf --version
python3 --version
pip3 --version
pipx --version
```

### 3.6 Optional Cleanup
```bash
sudo apt autoremove -y
```

---
## 4. Install Visual Studio Code and Connect to WSL

### 4.1 Install VS Code (Windows)
Install the Windows version from the official website.
Download: https://code.visualstudio.com/

### 4.2 Install Required Extensions
- WSL (or Remote Development pack)
- GitHub Copilot
- GitLens
- Recommended language extensions: Python, C/C++, TypeScript/JavaScript, Markdown, JSON tools.

### 4.3 Connect VS Code to WSL

1. Press **Ctrl+Shift+P** in Visual Studio Code.  
2. Run: **WSL: Connect to WSL using Distro...**.
3. From the list of WSL distributions, select **Ubuntu-24.04**.  
4. Once connected, open a folder **inside the Ubuntu filesystem**, such as:

   - Your home directory: `/home/<your-username>`  
   - Or your workspace: `/home/<your-username>/repos/ai-assisted-dev-stack`  

Opening a folder under `/home/...` keeps all code and tools running on the Linux side of WSL, which improves performance and avoids path and permission issues that can occur when working directly out of `/mnt/c` or other Windows-mounted locations.

### 4.4 Verify VS Code Server
```bash
ls ~/.vscode-server
```

### 4.5 Optional: Enable Settings Sync

If you want Visual Studio Code to sync extensions, settings, keybindings, and UI preferences across machines:

1. Click the **Settings (gear) icon** in the lower-left corner of VS Code.  
2. Select **Backup and Sync Settings…**.
3. Click **Sign in** under the *Settings Sync* section.  
4. Choose to sign in with either **GitHub** or **Microsoft**.  
5. After authentication, select which items you want to sync (settings, extensions, keybindings, snippets, UI state, etc.).  

Enabling Settings Sync is optional and does not affect the functionality of the development environment, but it helps maintain consistency across machines.

---
## 5. Configure Git and Install GitHub CLI

### 5.1 Configure Git Identity
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

### 5.2 Install GitHub CLI
(Add GPG key, repo, then install)

```bash
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh -y
```

### 5.3 Authenticate GitHub CLI

Run the authentication command:

```bash
gh auth login
```

When prompted, choose the following options:

Where do you use GitHub?  
Select: GitHub.com

What is your preferred protocol for Git operations on this host?  
Select: HTTPS

Authenticate Git with your GitHub credentials?  
Type: Y and press enter

How would you like to authenticate GitHub CLI?  
Select: Login with a web browser

After making these selections, GitHub CLI will show a one-time code and attempt to open the GitHub device login page in your browser. If your browser does not open automatically inside WSL, manually open your Windows browser and go to:

https://github.com/login/device

Enter the one-time code shown in your terminal to complete authentication.

### 5.4 Optional: SSH Git Setup
```bash
gh auth setup-git
```

### 5.5 Verify
```bash
gh repo list --limit 5
```

---
## 6. Create Workspace and Clone Repository

### 6.1 Create Workspace
```bash
mkdir -p ~/repos
cd ~/repos
```

### 6.2 Clone Repo
```bash
gh repo clone randyjreid/ai-assisted-dev-stack
cd ~/repos/ai-assisted-dev-stack
```

### 6.3 Open in VS Code
```bash
code ~/repos/ai-assisted-dev-stack
```

---
## 7. Enable GitHub Copilot

Copilot works entirely inside Visual Studio Code.

### 7.1 Confirm Extension
Ensure GitHub Copilot extension is installed.

### 7.2 Verify Suggestions
Start typing code and confirm inline suggestions appear.

### 7.3 Optional: Copilot Chat
Open Copilot Chat panel.

---
## 8. Install and Configure Claude Code CLI

Claude Code CLI provides reasoning with repository awareness, editing across multiple files, and structured development workflows.

### 8.1 Install Claude Code CLI

Install Claude Code CLI inside WSL Ubuntu using the official installer:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

This installs the `claude` executable and updates your PATH. If the command is not found immediately afterward, open a new WSL terminal or run:

```bash
source ~/.bashrc
```

### 8.2 Log Into Claude Code CLI

Authenticate your local CLI with your Anthropic account:

```bash
claude login
```

This opens a browser window (or gives you a device code and URL to open manually). Once approved, the CLI stores a secure local session and no API key is required.

If your browser does not open automatically, copy the device URL into a Windows browser and complete the login manually.

### 8.3 Verify Installation

Verify that the CLI installed correctly:

```bash
claude --help
```

Run the built‑in diagnostic tool:

```bash
claude update
claude doctor
```

These commands should display version information and confirm that the CLI is healthy.

### 8.4 Launch Claude Code in a Project

To open Claude Code inside a repository, navigate to the project directory:

```bash
cd ~/repos/ai-assisted-dev-stack
claude
```

Claude Code will start an interactive session scoped to that project, enabling reasoning across files, edits, tasks, and development workflows.

---
## 9. Install and Configure OpenAI Codex CLI

The OpenAI Codex CLI is a coding agent that runs directly in the terminal.
It is separate from the general purpose OpenAI API CLI and is installed using Node.js (via npm), not Python.
Codex provides repository awareness, code generation, analysis, and structured interactive development workflows.

Because Ubuntu 24.04 manages system Node packages tightly, Codex should be installed using **nvm (Node Version Manager)** to avoid permissions issues and system conflicts.

---
### 9.1 Install Node Version Manager (nvm)

Install nvm:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

Reload your shell so nvm is available:

```bash
source ~/.bashrc
```

Verify nvm is installed:

```bash
nvm --version
```

---

### 9.2 Install Node.js (LTS) using nvm

Install the latest LTS release:

```bash
nvm install --lts
```

Verify Node.js and npm:

```bash
node -v
npm -v
```

---

### 9.3 Install the OpenAI Codex CLI

Install Codex globally via npm (no sudo required):

```bash
npm install -g @openai/codex
```

Verify installation:

```bash
codex --help
```

The help menu should appear.

---

### 9.4 Log Into Codex

Authenticate your Codex CLI with your OpenAI account:

```bash
codex login
```

If your browser does not open automatically (WSL interop disabled), open the displayed URL manually in your Windows browser and enter the device code.

Codex is available to users with ChatGPT Plus, Pro, Team, Business, or Enterprise plans.  
Once authenticated, the CLI stores a secure local session.

---

### 9.5 Verify Authentication

Check your status:

```bash
codex status
```

You should see the linked OpenAI account and plan.

---

### 9.6 Launch Codex Inside a Project

Navigate to your project directory:

```bash
cd ~/repos/ai-assisted-dev-stack
codex
```

Codex will start an interactive session scoped to the current project, enabling reasoning with file awareness, code edits, refactoring, explanations, and development assistance.

---
## 10. Optional and Future Components

Additional tools and enhancements are planned for later phases, including Docker containers, Python environment managers, GitHub Actions automation, local LLM runtimes, and other productivity tools.

For a complete list of planned and potential future components, see [`roadmap.md`](roadmap.md).

---
## 11. Summary

This guide installs a complete, reproducible AI Assisted Dev Stack built on WSL Ubuntu 24.04, Visual Studio Code, GitHub tooling, GitHub Copilot, Claude Code CLI, and the OpenAI Codex CLI.
Together, these components create a modern development environment using multiple LLMs where AI tools assist with coding, analysis, refactoring, reasoning at the repository level, and interactive development workflows.

The result is a stable, extensible foundation that can be recreated on any machine, supports both cloud based and local tools, and enables a structured, professional workflow centered around version control, automation, and engineering with AI.
This environment forms the baseline for future phases, including automated bootstrapping, CI/CD integration, Dev Containers, and additional development tools in an agent style.
