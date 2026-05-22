# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

A collection of Markdown-based AI agent personality files for use with Claude Code and other AI assistants. There is no code to build, no test suite, and no dependencies — the entire repository is structured Markdown.

## Installing Agents

```bash
# Copy all agents to Claude Code's agents directory
cp -r agency-agents/* ~/.claude/agents/
```

## Repository Structure

Agents are organized by division into top-level directories. Each directory contains only `.md` agent files:

| Directory | Division |
|-----------|----------|
| `engineering/` | Software development specialists |
| `design/` | UX/UI and creative specialists |
| `marketing/` | Growth and marketing specialists |
| `product/` | Product management specialists |
| `project-management/` | PM and coordination specialists |
| `testing/` | QA and testing specialists |
| `support/` | Operations and support specialists |
| `spatial-computing/` | AR/VR/XR specialists |
| `specialized/` | Cross-cutting or unique specialists |
| `strategy/` | NEXUS orchestration doctrine, playbooks, and runbooks |
| `examples/` | Multi-agent collaboration examples |

## Agent File Anatomy

Every agent file must follow this exact structure:

```markdown
---
name: Agent Name
description: One-line description of the agent's specialty and focus
color: colorname or "#hexcode"
---

# Agent Name

## 🧠 Your Identity & Memory
## 🎯 Your Core Mission
## 🚨 Critical Rules You Must Follow
## 📋 Your Technical Deliverables
## 🔄 Your Workflow Process
## 💭 Your Communication Style
## 🔄 Learning & Memory
## 🎯 Your Success Metrics
## 🚀 Advanced Capabilities
```

The YAML frontmatter (`name`, `description`, `color`) is required and consumed by Claude Code when listing available agents.

## Naming Convention

Agent files are named `<category>-<agent-name>.md` using kebab-case, where category matches the directory name:

```
engineering/engineering-frontend-developer.md
testing/testing-reality-checker.md
specialized/agents-orchestrator.md   # exception: no prefix when category doesn't apply cleanly
```

## NEXUS Orchestration

`strategy/nexus-strategy.md` defines the multi-agent coordination doctrine. Key concepts:

- **NEXUS-Full**: All divisions, 6 phases (Discovery → Strategy → Foundation → Build → Harden → Launch → Operate)
- **NEXUS-Sprint**: Subset of agents for feature/MVP delivery
- **NEXUS-Micro**: 5–10 agents for scoped tasks (bug fix, campaign, audit)
- **Dev↔QA Loop**: Every implementation task passes through QA before proceeding; max 3 retries before escalation
- **Quality Gates**: Phase advancement requires evidence-based approval from the Reality Checker
- **Agents Orchestrator** (`specialized/agents-orchestrator.md`) is the pipeline controller that manages handoffs

The `strategy/playbooks/` directory contains per-phase activation instructions. The `strategy/runbooks/` directory contains scenario-specific instructions (startup MVP, marketing campaign, incident response, enterprise feature).

## Contributing an Agent

1. Choose the appropriate division directory (or `specialized/` if none fit)
2. Name the file `<category>-<role>.md`
3. Include all required frontmatter fields and all standard sections
4. Include concrete code/template examples with real, runnable code — not pseudo-code
5. Define specific, measurable success metrics (e.g., "Page load under 3 seconds on 3G", not "make it fast")
6. PR title format: `Add [Agent Name] - [Category]`
7. PRs must include the checklist from `.github/PULL_REQUEST_TEMPLATE.md`

## Agent Design Principles

- **Narrow scope**: Deep specialization beats breadth. Avoid jack-of-all-trades agents.
- **Strong personality**: Distinct voice and character, not "I am a helpful assistant"
- **Deliverable-first**: Every agent section should produce a concrete artifact — code, template, document, or decision
- **Evidence over claims**: Success metrics must be measurable; workflows must be battle-tested
