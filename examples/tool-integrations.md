# Integration Examples: Popular Tools

> **What this covers:** How to load Agency agents into the tools you already use — Claude Code, Cursor, GitHub Copilot, ChatGPT, Windsurf, and Aider. Each section shows the exact steps and any tool-specific configuration required.

---

## Table of Contents

1. [Claude Code (CLI)](#1-claude-code-cli)
2. [Cursor AI](#2-cursor-ai)
3. [GitHub Copilot](#3-github-copilot)
4. [ChatGPT (Custom GPTs & Projects)](#4-chatgpt-custom-gpts--projects)
5. [Windsurf](#5-windsurf)
6. [Aider](#6-aider)
7. [Mixing Agents Across Tools](#7-mixing-agents-across-tools)

---

## 1. Claude Code (CLI)

The Agency is designed first-class for Claude Code. Agents live in `~/.claude/agents/` and activate automatically when referenced by name.

### Setup

```bash
# One-time: copy all agents to your Claude Code agents directory
cp -r /path/to/agency-agents/* ~/.claude/agents/

# Or clone and symlink (keeps agents up to date)
git clone https://github.com/msitarzewski/agency-agents ~/agency-agents
ln -sf ~/agency-agents/engineering ~/.claude/agents/engineering
ln -sf ~/agency-agents/design ~/.claude/agents/design
# ... repeat for each division
```

### Activating an Agent

Reference any agent by name at the start of a session or mid-conversation:

```
> Activate the Frontend Developer agent. I need a React component that...
> Switch to Evidence Collector mode and audit the current UI for accessibility issues.
> You are now the Backend Architect. Review this API design and tell me what's wrong.
```

### Multi-Agent Workflow Example

Claude Code can run multiple agents in sequence or in parallel using the Agent SDK:

```bash
claude "Use the Sprint Prioritizer to sort this backlog, then hand the top 3 items \
  to the Senior Developer for implementation estimates, and have the Reality Checker \
  define the acceptance criteria."
```

### Scoping an Agent to a Project

Add a `CLAUDE.md` at your project root to auto-activate an agent for that repo:

```markdown
<!-- CLAUDE.md -->
# Project Configuration

You are operating as the **Senior Developer** agent (see ~/.claude/agents/engineering/engineering-senior-developer.md).
Apply all rules and workflows defined in that agent for every task in this project.
```

### Tips

- Use `/agents` in Claude Code to list all available agents
- Agents persist their identity for the full session — no need to re-activate
- Layer agents: start with Rapid Prototyper for speed, hand off to Senior Developer for hardening

---

## 2. Cursor AI

Cursor reads project-level and global rules files that inject agent personality as a persistent system context.

### Project-Level Agent (`.cursor/rules/agency.mdc`)

Create a rules file to activate an agent for an entire project:

```markdown
---
description: Agency agent active for this project
alwaysApply: true
---

<!-- .cursor/rules/agency.mdc -->

You are operating as the **Frontend Developer** agent from The Agency.

[Paste the full contents of engineering/engineering-frontend-developer.md here, 
excluding the YAML frontmatter block]
```

### Global Agent (User Rules)

To activate an agent globally across all Cursor projects:

1. Open **Cursor Settings → Rules for AI**
2. Paste the agent content (excluding frontmatter) into the "User Rules" field
3. The agent identity applies to every conversation until you change it

### Switching Agents Per File Type

Use Cursor's glob-pattern rules to apply different agents to different contexts:

```markdown
---
description: Backend agent for server-side files
globs: ["src/api/**", "src/server/**", "*.service.ts"]
alwaysApply: false
---

You are the **Backend Architect** agent. [paste agent content]
```

```markdown
---
description: Frontend agent for UI files  
globs: ["src/components/**", "src/pages/**", "*.tsx"]
alwaysApply: false
---

You are the **Frontend Developer** agent. [paste agent content]
```

### Composer Multi-Agent Workflow

In Cursor Composer, run agents in sequence:

```
1. [Sprint Prioritizer]: Given this feature list, rank the top 5 by user impact × dev effort.
2. [Rapid Prototyper]: Build a working prototype of item #1.
3. [Reality Checker]: Audit the prototype. What's missing before shipping?
```

---

## 3. GitHub Copilot

GitHub Copilot reads a custom instructions file to adjust its behavior per-repository.

### Repository Instructions (`.github/copilot-instructions.md`)

```markdown
<!-- .github/copilot-instructions.md -->

## Active Agent: Senior Developer

You are operating as the **Senior Developer** from The Agency — a specialist in
Laravel/Livewire, advanced architecture patterns, and complex implementations.

### Your approach in this repository:
- Prefer composition over inheritance; use service classes, not fat controllers
- Every method should have a single, clear responsibility
- Use Laravel conventions: Eloquent scopes, form requests, resource controllers
- Default to typed properties and return types everywhere

### What you produce:
- Production-ready code with full error handling
- Migration files with rollback support
- Feature tests with Pest for every new endpoint
- PHPDoc for all public methods

### You will not:
- Skip validation on any user input
- Write raw SQL when Eloquent can handle it
- Leave TODO comments — finish the implementation or file an issue
```

### Copilot Chat Agent Mode

In VS Code with GitHub Copilot Chat, paste the agent content as a custom instruction at the start of a chat session:

```
#file:.github/copilot-instructions.md

Now, with the Senior Developer agent active, review src/Services/OrderService.php 
and refactor it to use the repository pattern.
```

### Per-Language Instructions

Copilot also supports language-specific instructions in VS Code settings:

```json
// .vscode/settings.json
{
  "github.copilot.chat.codeGeneration.instructions": [
    {
      "file": ".github/copilot-instructions.md"
    }
  ]
}
```

---

## 4. ChatGPT (Custom GPTs & Projects)

### Custom GPT Setup

1. Go to **ChatGPT → Explore GPTs → Create**
2. In the **Instructions** field, paste the agent content (excluding frontmatter)
3. Set the GPT name to match the agent (e.g., "Growth Hacker")
4. Optionally upload relevant knowledge files (your codebase, brand guidelines, etc.)
5. Save and use the GPT for targeted sessions

**Example: Growth Hacker Custom GPT**

```
Instructions:
You are the Growth Hacker from The Agency.

[Paste contents of marketing/marketing-growth-hacker.md here]

Additional context for this GPT:
- Our product is a B2B SaaS tool for engineering teams
- Current MRR: $45k, target: $200k in 6 months
- Primary acquisition channel today: organic SEO
```

### ChatGPT Projects

ChatGPT Projects let you maintain persistent agent context across conversations:

1. Create a new Project named after your agent ("UX Researcher", "Backend Architect")
2. In Project Instructions, paste the agent content
3. Upload any reference files (design system docs, API specs, etc.)
4. All conversations in the project inherit the agent personality

### Temporary Agent Activation (Any Chat)

For one-off sessions without creating a GPT or Project:

```
System: You are the Evidence Collector from The Agency — an expert QA specialist 
who requires visual proof for every bug and defaults to finding 3-5 issues per review.

[Paste agent content here]

---

User: Review these screenshots of our checkout flow and identify all UX issues.
```

---

## 5. Windsurf

Windsurf (by Codeium) reads a `.windsurfrules` file at the repository root.

### Project Agent (`.windsurfrules`)

```markdown
<!-- .windsurfrules -->

## Active Agency Agent: DevOps Automator

You are the **DevOps Automator** from The Agency — an expert in CI/CD pipelines,
infrastructure automation, and cloud operations.

[Paste contents of engineering/engineering-devops-automator.md here, 
excluding the YAML frontmatter]

## Repository Context

- Cloud provider: AWS (ECS Fargate, RDS Aurora, CloudFront)
- IaC: Terraform (modules in ./infrastructure/)
- CI/CD: GitHub Actions (workflows in ./.github/workflows/)
- Container registry: ECR
- Environments: dev, staging, prod (manual approval gate before prod)
```

### Cascade Multi-Step Workflows

Windsurf's Cascade can chain agents by describing the handoff explicitly:

```
Step 1 (Rapid Prototyper): Build a working OAuth2 integration in under 2 hours.
  → Deliver: Working code, no polish required.

Step 2 (Senior Developer): Take the prototype above and harden it for production.
  → Deliver: Typed interfaces, full error handling, test coverage, security review.

Step 3 (Evidence Collector): Audit the final implementation.
  → Deliver: Screenshot evidence of each auth flow + list of any remaining issues.
```

### Global Rules

For a global agent across all Windsurf projects, add agent content to:
**Windsurf Settings → AI → Custom Instructions**

---

## 6. Aider

Aider is an open-source CLI coding agent that works with any LLM. Use a conventions file to inject agent personality.

### Agent Convention File (`CONVENTIONS.md`)

```markdown
<!-- CONVENTIONS.md — loaded automatically by aider -->

## Active Agent: AI Engineer

You are the **AI Engineer** from The Agency — a specialist in ML model integration,
data pipelines, and AI-powered application development.

[Paste contents of engineering/engineering-ai-engineer.md here]
```

### Launch Aider with Agent Context

```bash
# Load agent conventions automatically
aider --read CONVENTIONS.md

# Or specify the agent file directly
aider --read engineering/engineering-ai-engineer.md

# Multi-model: use Opus for architecture decisions, Sonnet for implementation
aider --model claude-opus-4-7 --read CONVENTIONS.md
```

### Switching Agents Mid-Session

```bash
# In an aider session, drop and reload conventions
/drop CONVENTIONS.md
/read design/design-ux-architect.md
```

### Automated Agent Pipelines with Aider

```bash
#!/bin/bash
# Run multiple agents sequentially on the same codebase

# Phase 1: Architect the feature
aider --read engineering/engineering-backend-architect.md \
      --message "Design the data model and API for user notifications" \
      --yes

# Phase 2: Implement it
aider --read engineering/engineering-senior-developer.md \
      --message "Implement the notification system per the design above" \
      --yes

# Phase 3: Verify it
aider --read testing/testing-reality-checker.md \
      --message "Audit the notification implementation for production readiness" \
      --yes
```

---

## 7. Mixing Agents Across Tools

The most powerful workflows use different tools for what each does best.

### Example: Full Feature Lifecycle

| Phase | Agent | Tool | Why |
|-------|-------|------|-----|
| Discovery | Product Trend Researcher | ChatGPT Project | Long research threads with document uploads |
| Architecture | Backend Architect | Claude Code | Deep codebase access, file editing |
| Implementation | Senior Developer | Cursor | Inline completions during active coding |
| PR Review | Reality Checker | Claude Code | Access to git diff and test output |
| Documentation | Content Creator | ChatGPT | Writing-focused, no code context needed |

### Handoff Pattern

Each agent should end its work with a structured handoff note that the next agent can consume:

```markdown
## Handoff: Backend Architect → Senior Developer

**What I designed:**
- 3-table schema (users, notifications, notification_preferences)
- REST API: POST /notifications, GET /notifications/:userId, PATCH /notifications/:id/read
- Background job: NotificationDispatcher (runs every 60s)

**Key decisions made:**
- Soft deletes on notifications (never hard-delete for audit trail)
- Fan-out on write (pre-compute per-user feeds at write time, not read time)
- Rate limiting: 100 notifications/user/hour enforced at API layer

**What's left for implementation:**
- [ ] All model/migration files
- [ ] Controller implementation
- [ ] Background job
- [ ] Unit tests (target: 90% coverage on business logic)
- [ ] API documentation

**Watch out for:**
- The preferences table has a JSONB `channels` column — validate strictly, don't trust input
```

### Agent Continuity Across Sessions

When switching tools or resuming a long project, prepend context for the incoming agent:

```
You are the Evidence Collector agent.

Previous session summary:
- Sprint Prioritizer ranked notifications as the #1 feature
- Backend Architect designed the schema (see ARCHITECTURE.md)
- Senior Developer implemented it (see PR #47)

Your task: Audit PR #47 against the acceptance criteria in ARCHITECTURE.md.
Flag any gaps as blocking issues. Produce screenshot evidence for each.
```
