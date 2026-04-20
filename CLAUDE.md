# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains the **AI-Accelerated Software Development Workshop setup guide** (in Thai). The main content is `README.md` which provides universal setup instructions for Windows/macOS/Linux, including WSL2 configuration, Bun runtime, and Claude Code CLI installation.

## Skills Templates

The `downloads/` directory contains skill templates that can be imported into Claude Code. These are specialized subagent configurations.

### Skill Format

Skills must include YAML frontmatter with these fields:

```yaml
---
name: skill-name
description: Single-line description for when to trigger this skill
allowed-tools: [Read, Grep, Bash]  # Tools the skill can use
---
```

### Adding New Skills

1. Create a new directory in `downloads/skill-name/`
2. Add a `SKILL.md` file with the skill instructions and YAML frontmatter
3. Skills should follow the pattern of existing templates:
   - `doc_writer.md` — Technical documentation specialist
   - `security-scan-list/SKILL.md` — Security audit based on CWE/OWASP criteria

### Naming Conventions

- Directory names: `kebab-case`
- Skill `name` field: `kebab-case`
- Skill files: `SKILL.md` (uppercase) inside skill directory

## Package Managers

Target platforms: **Ubuntu 24.04** and **macOS**. Regular `pip` is NOT used — these systems require isolated package managers:

| Tool | Purpose | Install/Run Command |
|------|---------|---------------------|
| **npm** | Node.js package manager | Pre-installed with Node.js (via fnm) |
| **npx** | Run Node packages without installing | `npx <package>` |
| **pipx** | Install Python CLI tools in isolated venvs | `pipx install <package>` |
| **uv** | Fast Python package installer (modern pip replacement) | `uv pip install` or `uv tool install` |
| **uvx** | Run Python tools without installing (like npx for Python) | `uvx <package>` |

**Why these tools?**
- Ubuntu 24.04 and macOS require `pipx` for system-level Python tools to avoid conflicts
- `uv`/`uvx` are preferred over pip for speed and reliability
- `npx` allows running Node packages globally without polluting the system

**Installation reference (from README.md):**
```bash
# Ubuntu/WSL
sudo apt install -y pipx
pipx ensurepath
curl -LsSf https://astral.sh/uv/install.sh | sh

# macOS
brew install pipx
pipx ensurepath
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Language

The README.md is in **Thai** (ภาษาไทย). When editing documentation, maintain Thai language and follow the existing structure with images, code blocks, and collapsible sections.
