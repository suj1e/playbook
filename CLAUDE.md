# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a **personal knowledge base** ("Playbook") for software engineering fundamentals, operations, architecture patterns, and best practices. The repository contains documentation (Markdown) organized by topic and a sophisticated Claude Code AI assistant configuration.

**Important:** The actual playbook content (fundamentals/, manuals/, playbooks/, patterns/, checklists/, tools/) is planned but not yet created. Currently, only the `.claude/` configuration directory exists.

## Directory Structure

```
.claude/               # Claude Code AI assistant configuration
├── agents/           # Specialized AI agents (backend, devops, ai, architect-review, prompt-engineer)
├── commands/         # Custom slash commands (/architecture-review, /ultra-think, /update-docs)
├── scripts/          # Utility scripts (context-monitor.py for token usage)
└── skills/           # Reusable skills (git-pushing for automated commits)

fundamentals/         # (Planned) CS fundamentals: networking, OS, databases, distributed systems
manuals/              # (Planned) Operation manuals: Git, Linux, Docker, Kubernetes, Cloud
playbooks/            # (Planned) Core: system design, microservices, performance, troubleshooting
patterns/             # (Planned) Architecture patterns: backend, frontend, distributed
checklists/           # (Planned) Checklists: deployment, incident response, migration
tools/                # (Planned) Tool configs: Neovim, LazyVim, Wezterm, Zellij, CLI
drafts/               # (Planned) Work-in-progress content
```

## Git Workflow

Use the **git-pushing skill** for all commits and pushes. Do NOT use manual git commands:

```bash
# Standard commit and push (auto-generates conventional commit message)
bash skills/git-pushing/scripts/smart_commit.sh

# Custom commit message
bash skills/git-pushing/scripts/smart_commit.sh "feat: add new documentation"
```

The script handles: staging, conventional commit message format, Claude co-author footer, and push with `-u` flag.

The skill automatically activates when users say phrases like "push this", "commit and push", "save to github", etc.

## Available Commands

- `/architecture-review` - Comprehensive architecture review with design patterns analysis (uses architect-review agent)
- `/ultra-think` - Deep multi-dimensional problem analysis and solution synthesis
- `/update-docs` - Systematic documentation updates across the project
- `/git-pushing` - Automated git commit and push workflow (activates automatically)

## Specialized Agents

The repository includes several specialized AI agents that can be invoked via the Task tool:

- **backend-architect** - RESTful APIs, microservices, database schemas, scalability (uses Sonnet)
- **devops-engineer** - CI/CD pipelines, Terraform, Kubernetes, monitoring, infrastructure (uses Sonnet)
- **ai-engineer** - LLM integration, RAG systems, vector databases, agent frameworks (uses Opus)
- **architect-review** - Architecture reviews, SOLID principles, pattern adherence (uses Opus)
- **prompt-engineer** - LLM prompt optimization, chain-of-thought, few-shot learning (uses Opus)

## Context Monitoring

A Python script at `.claude/scripts/context-monitor.py` provides real-time token usage visualization for Claude Code sessions. Run it to monitor context consumption during long sessions.

## Content Guidelines

When creating documentation for this playbook:

1. **Language:** Content should be bilingual (English and Chinese) to match the existing README
2. **Format:** Use Markdown for all documentation
3. **Structure:** Follow the planned directory structure outlined in README.md
4. **Purpose:** Focus on practical knowledge - fundamentals, how-to guides, implementation patterns, and checklists

## Development Notes

- No build system, package manager, or testing framework - this is a documentation-only repository
- Git permissions are pre-configured in `.claude/settings.local.json` for common operations
- The status line is customized to show model name, directory, git branch, and change count
