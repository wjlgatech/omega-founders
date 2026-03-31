# Claude Certified Architect — Study Resources Map

## Primary Resources

### 1. Anthropic Academy (Skilljar) — FREE
**URL**: https://anthropic.skilljar.com/

Courses mapped to exam domains:

| Course | Hours | Exam Domains Covered |
|--------|-------|---------------------|
| Building with the Claude API | 8.1 hrs | D1, D2, D4, D5 |
| Introduction to Model Context Protocol | ~2 hrs | D2 |
| Model Context Protocol: Advanced Topics | ~3 hrs | D2, D3 |
| Claude Code in Action | ~3 hrs | D3 |
| Introduction to Agent Skills | ~2 hrs | D1, D3 |

**Study strategy**: Don't watch linearly. Map each course section to a specific exam task statement and watch only what fills knowledge gaps.

### 2. Claude Code Official Docs
**Base URL**: https://code.claude.com/docs

#### Domain 1 — Agentic Architecture (27%)
| Doc Page | Task Statements |
|----------|----------------|
| [Sub-agents](https://code.claude.com/docs/en/sub-agents.md) | 1.2, 1.3 |
| [Agent teams](https://code.claude.com/docs/en/agent-teams.md) | 1.2, 1.6 |
| [Hooks guide](https://code.claude.com/docs/en/hooks-guide.md) | 1.4, 1.5 |
| [Hooks reference](https://code.claude.com/docs/en/hooks.md) | 1.5 |
| [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works.md) | 1.1 |

#### Domain 2 — Tool Design & MCP (18%)
| Doc Page | Task Statements |
|----------|----------------|
| [MCP integration](https://code.claude.com/docs/en/mcp.md) | 2.4 |
| [Tools reference](https://code.claude.com/docs/en/tools-reference.md) | 2.1, 2.5 |
| [Plugins](https://code.claude.com/docs/en/plugins.md) | 2.3, 2.4 |
| [Plugins reference](https://code.claude.com/docs/en/plugins-reference.md) | 2.3 |

#### Domain 3 — Claude Code Config & Workflows (20%)
| Doc Page | Task Statements |
|----------|----------------|
| [Memory (CLAUDE.md)](https://code.claude.com/docs/en/memory.md) | 3.1 |
| [.claude directory](https://code.claude.com/docs/en/claude-directory.md) | 3.1, 3.2 |
| [Skills](https://code.claude.com/docs/en/skills.md) | 3.2 |
| [Built-in commands](https://code.claude.com/docs/en/commands.md) | 3.2 |
| [Settings](https://code.claude.com/docs/en/settings.md) | 3.3 |
| [Best practices](https://code.claude.com/docs/en/best-practices.md) | 3.4, 3.5 |
| [GitHub Actions](https://code.claude.com/docs/en/github-actions.md) | 3.6 |
| [GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd.md) | 3.6 |
| [Headless mode](https://code.claude.com/docs/en/headless.md) | 3.6 |
| [CLI reference](https://code.claude.com/docs/en/cli-reference.md) | 3.4, 3.6 |

#### Domain 4 — Prompt Engineering & Structured Output (20%)
| Doc Page | Task Statements |
|----------|----------------|
| [Common workflows](https://code.claude.com/docs/en/common-workflows.md) | 4.1, 4.2 |
| [Output styles](https://code.claude.com/docs/en/output-styles.md) | 4.3 |

#### Domain 5 — Context Management & Reliability (15%)
| Doc Page | Task Statements |
|----------|----------------|
| [Context window](https://code.claude.com/docs/en/context-window.md) | 5.x |
| [Checkpointing](https://code.claude.com/docs/en/checkpointing.md) | 5.x |
| [Permissions](https://code.claude.com/docs/en/permissions.md) | 5.x |
| [Sandboxing](https://code.claude.com/docs/en/sandboxing.md) | 5.x |
| [Monitoring](https://code.claude.com/docs/en/monitoring-usage.md) | 5.x |

### 3. Claude API / Platform Docs
**Base URL**: https://docs.anthropic.com (redirects to platform.claude.com)

Key pages for exam prep:
- Messages API (tool_use, stop_reason, structured output)
- Tool use guide (tool_choice, JSON schemas)
- Message Batches API (batch processing, custom_id)
- Prompt engineering guide (few-shot, system prompts)
- Agent SDK overview (AgentDefinition, Task tool, hooks)

### 4. Community Resources
| Resource | URL | Notes |
|----------|-----|-------|
| Claude Certifications (study guide + practice Qs) | https://claudecertifications.com/ | Free practice questions |
| FlashGenius Guide | https://flashgenius.net/blog-article/a-guide-to-the-claude-certified-architect-foundations-certification | Detailed walkthrough |
| AI Corner Curriculum | https://www.the-ai-corner.com/p/claude-certified-architect-curriculum-2026 | Full curriculum map |
| Analytics Vidhya (free courses list) | https://www.analyticsvidhya.com/blog/2026/03/free-anthropic-ai-courses-with-certificates/ | 7 free courses with certs |

---

## Daily Resource Schedule

| Day | Domain | Primary Docs | Skilljar Course Section |
|-----|--------|-------------|------------------------|
| 1 | D1: Agentic Loops | how-claude-code-works, sub-agents | Building with Claude API: Agents |
| 2 | D1: Multi-Agent | agent-teams, sub-agents | Agent Skills course |
| 3 | D2: Tool Design & MCP | mcp, tools-reference, plugins | MCP Intro + Advanced |
| 4 | D1: Hooks & Enforcement | hooks-guide, hooks-reference | Building with Claude API: Tools |
| 5 | D3: CLAUDE.md & Config | memory, claude-directory, skills | Claude Code in Action |
| 6 | D3: Plan Mode & CI/CD | best-practices, github-actions, headless | Claude Code in Action |
| 7 | D4: Prompts & Structured | common-workflows, output-styles | Building with Claude API: Prompts |
| 8 | D4: Validation & Batch | (API docs: batches, tool_use) | Building with Claude API: Advanced |
| 9 | D5: Context & Reliability | context-window, checkpointing, monitoring | Building with Claude API: Context |
| 10 | ALL: Scenario Practice | Review all weak areas | Practice exams |
