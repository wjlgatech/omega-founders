# 10-Day Claude Architect Certification: Study × Build × Teach Plan

**Start Date**: March 29, 2026 (Saturday)
**Exam Target**: April 7, 2026 (Monday) or earlier
**Daily Time**: 2 hours (min) to 3 hours (max)
**Daily Deadline**: Start before 9 AM PT or get texted

---

## THE 7 CORE AI PROFESSIONAL SKILLS
*The practitioner lens that sits on top of the exam. Every day you're building both the cert and these.*

| Skill | Exam Home | Days |
|---|---|---|
| **1. Specification Precision** — eliminate ambiguity, "talk English to a machine" | D4, D3 | 5, 7 |
| **2. Evaluation & Quality Judgment** — AI is "fluently wrong"; your name is on the output | D4, D5 | 7, 8, 9 |
| **3. Decomposing Tasks & Delegating** — managerial mindset for multi-agent systems | D1 | 2, 6 |
| **4. Failure Pattern Recognition** — 6 specific failure modes, not generic "bugs" | D1, D5 | 1, 4, 9 |
| **5. Trust & Security Design** — cost of error, blast radius, reversibility test | D1, D2 | 3, 4 |
| **6. Context Architecture** — persistent vs per-session context, the "librarian" skill | D5, D1 | 9 |
| **7. Cost & Token Economics** — ROI proof, blended model cost, batch savings | D4 | 8 |

---

## THE 6 AI FAILURE MODES
*Learn to recognize these by name. They appear as exam distractors and real production disasters.*

| # | Failure Mode | What Happens | Exam Domain |
|---|---|---|---|
| 1 | **Context Degradation** | Performance drops as session lengthens — context window gets "polluted" | D5 |
| 2 | **Specification Drift** | Agent "forgets" its original instructions mid-task | D4, D5 |
| 3 | **Sycophantic Confirmation** | Agent confirms your wrong data and builds a faulty system around it | D4, D5 |
| 4 | **Tool Selection Errors** | Agent picks the wrong tool — too many tools, bad descriptions, broken harness | D2 |
| 5 | **Cascading Failure** | One agent's error propagates through the whole multi-agent chain | D1 |
| 6 | **Silent Failure** ⚠️ MOST DANGEROUS | Output *looks* correct but is wrong — hardest to detect, hardest to root cause | D4, D5 |

---

## Day 1 (Mar 29) — AGENTIC LOOPS: The Heartbeat of Claude Agents

**Domain**: D1 — Agentic Architecture & Orchestration (Task Statements 1.1, 1.6)
**Real-World Skill Built**: Failure Pattern Recognition (#4)
**Failure Modes Active**: Silent Failure (#6), Tool Selection Errors (#4)

### LEARN (45 min)
- The agentic loop lifecycle: send request → inspect `stop_reason` → execute tool → return result
- `stop_reason: "tool_use"` = keep going; `stop_reason: "end_turn"` = done
- Tool results MUST be appended to conversation history (not replaced) — Claude reasons from the full history
- Model-driven vs pre-configured decision trees (why model-driven wins for ambiguity)
- **3 Anti-Patterns** (the exam will put these in distractors):
  - ❌ Parsing natural language to determine loop termination
  - ❌ Arbitrary iteration cap as the *primary* stopping mechanism (safety cap = fine; primary signal = wrong)
  - ❌ Checking assistant text content as completion indicator
- **Practitioner lens**: The 3 anti-patterns ARE failure modes — they cause Silent Failure (looks like it ran, silently wrong)

### BUILD (60 min)
- **Skill skeleton**: Create `.claude/skills/claude-architect-mastery/SKILL.md`
- **Module 1**: `agentic-loops` — Interactive concept explainer
  - Simulates a tool-use loop step by step
  - Quiz: "Given this stop_reason, what should the loop do next?"
  - Code example: minimal agentic loop in Python using Claude SDK

### TEACH (30 min)
- **Blog post**: "The Agentic Loop: Claude's Heartbeat Explained in 60 Seconds"
  - Hook: "Every Claude agent is basically a while loop with a brain"
  - Core insight: The model decides what to do next, not your code
  - Diagram: the loop as a flowchart
- **YouTube short script**: 60-sec explainer of stop_reason
- **LinkedIn**: "Day 1 of my Claude Architect certification sprint..."

### REFLECT (15 min)
- 5 practice questions on agentic loops
- Map each anti-pattern to its failure mode (Silent Failure or Tool Selection Error?)
- What tripped you up? → feed back into skill

---

## Day 2 (Mar 30) — MULTI-AGENT ORCHESTRATION: The Coordinator Pattern

**Domain**: D1 — Agentic Architecture (Task Statements 1.2, 1.3)
**Real-World Skill Built**: Decomposing Tasks & Delegating (#3)
**Failure Modes Active**: Cascading Failure (#5), Tool Selection Errors (#4)

### LEARN (45 min)
- Hub-and-spoke: coordinator manages ALL inter-subagent communication
- Subagents have ISOLATED context — they don't inherit coordinator's history
- `Task` tool for spawning; `allowedTools` must include `"Task"` for nesting
- `AgentDefinition`: descriptions, system prompts, tool restrictions per subagent
- `fork_session` for divergent exploration from shared baseline
- **Risk**: overly narrow task decomposition → incomplete coverage (exam-tested)
- **Cascading Failure prevention**: coordinator is the circuit breaker — if a subagent fails, the coordinator catches it; no direct subagent-to-subagent paths = no cascades
- **Practitioner lens**: the "planner agent" pattern from real job specs maps exactly to coordinator + task decomposition

### BUILD (60 min)
- **Module 2**: `multi-agent-patterns` in the skill
  - Interactive scenario: "You're building a research system. Design the agent topology."
  - Quiz: "Subagent X needs data from Subagent Y. How does it get it?" (Answer: through the coordinator)
  - Code: coordinator agent that dynamically selects which subagents to invoke

### TEACH (30 min)
- **Blog post**: "Why Claude Agents Can't Talk to Each Other (And Why That's Good)"
  - Hook: "Imagine a team where every message goes through the manager. Sounds awful? For AI agents, it's genius."
  - Insight: Isolation = reliability. Coordinator = observability. No direct paths = no cascades.
- **YT short**: Hub-and-spoke animation concept
- **X thread**: 5-tweet breakdown of coordinator patterns

### REFLECT (15 min)
- 5 practice questions on multi-agent orchestration
- Update skill with any gaps discovered

---

## Day 3 (Mar 31) — TOOL DESIGN & MCP: The Agent's Hands

**Domain**: D2 — Tool Design & MCP Integration (Task Statements 2.1-2.5)
**Real-World Skill Built**: Trust & Security Design (#5) — tool descriptions as guardrails
**Failure Modes Active**: Tool Selection Errors (#4)

### LEARN (45 min)
- Tool descriptions are THE primary mechanism for LLM tool selection — vague = misrouting
- Before/after: `analyze_content` (vague, breaks) vs `analyze_customer_support_ticket` (precise, works)
- Structured error responses: `isError`, `errorCategory`, `isRetryable`
- Tool distribution: 4-5 tools per agent optimal; 18 = chaos (Tool Selection Error waiting to happen)
- `tool_choice`: `"auto"` / `"any"` / forced `{"type": "tool", "name": "..."}`
- MCP scoping: `.mcp.json` (project) vs `~/.claude.json` (user)
- Built-in tools: Grep (content search), Glob (file patterns), Read/Write/Edit, Bash
- **Trust & Security lens**: Every tool is an action with a blast radius. Design descriptions so the agent only reaches for the right tool in the right context.
- **Tool Selection Error prevention**: fewer tools + better descriptions + proper harness = reliable selection

### BUILD (60 min)
- **Module 3**: `tool-design-mcp` in the skill
  - "Tool Description Doctor": paste a bad tool description, get it rewritten
  - Quiz: "Which tool_choice setting guarantees the model calls a tool?"
  - MCP config generator: interactive `.mcp.json` builder
  - Error response designer: structured error metadata template

### TEACH (30 min)
- **Blog post**: "Your AI Agent Picks the Wrong Tool? It's Your Description's Fault"
  - Hook: "Tool descriptions for LLMs are like job postings for humans — vague ones attract the wrong candidates"
  - Practical: before/after of a bad vs good tool description
- **YT short**: "3 Rules for Tool Descriptions That Don't Suck"
- **LinkedIn**: MCP config tip of the day

### REFLECT (15 min)
- 5 practice questions on tool design and MCP
- Update mastery tracker

---

## Day 4 (Apr 1) — HOOKS, WORKFLOWS & ENFORCEMENT: The Guardrails

**Domain**: D1 — Agentic Architecture (Task Statements 1.4, 1.5, 1.7)
**Real-World Skill Built**: Trust & Security Design (#5) — reversibility, blast radius, cost of error
**Failure Modes Active**: Sycophantic Confirmation (#3), Silent Failure (#6)

### LEARN (45 min)
- Programmatic enforcement (hooks) vs prompt-based guidance
- **The key distinction**: Prompts = probabilistic. Hooks = deterministic. When business rules MUST hold → use hooks.
- `PostToolUse` hooks: intercept tool results for transformation/normalization
- Hook patterns: block refunds > $500, enforce identity verification before financial ops
- Structured handoff protocols: customer ID, root cause, recommended action
- Session management: `--resume`, `fork_session`, when to start fresh vs resume
- **Trust & Security lens**:
  - Reversibility test: "Can the user undo this?" Email draft = reviewable. Wire transfer = final. Hook required for final actions.
  - Blast radius analysis: "What's the worst-case if this fires incorrectly?"
  - "Be nice" in a system prompt is NOT a guardrail. It's a suggestion.
- **Sycophantic Confirmation defense**: Hooks don't confirm, they enforce. If the agent "agrees" that a $600 refund is valid, the hook blocks it regardless.

### BUILD (60 min)
- **Module 4**: `enforcement-patterns` in the skill
  - Hook template generator: "What business rule? → Here's your hook code"
  - Quiz: "Prompt instruction vs hook — which guarantees compliance?"
  - Scenario: Customer support agent with prerequisite gates
  - Session decision tree: resume vs fresh start vs fork

### TEACH (30 min)
- **Blog post**: "Prompts Lie, Hooks Don't: Deterministic Guardrails for AI Agents"
  - Hook: "If your AI agent handles money, a prompt saying 'don't exceed $500' is a suggestion, not a law"
  - Insight: Hooks = compiled rules. Prompts = interpreted suggestions.
- **YT short**: "The One Thing That Makes AI Agents Production-Safe"
- **X thread**: Hooks vs prompts decision framework + reversibility test

### REFLECT (15 min)
- 5 practice questions on enforcement and session management
- Full Domain 1 review — all 7 task statements
- Run the reversibility test on 3 real tools you've built or used

---

## Day 5 (Apr 2) — CLAUDE CODE CONFIG: The Developer's Operating System

**Domain**: D3 — Claude Code Configuration & Workflows (Task Statements 3.1, 3.2, 3.3)
**Real-World Skill Built**: Specification Precision (#1) — CLAUDE.md IS your specification document
**Failure Modes Active**: Specification Drift (#2)

### LEARN (45 min)
- CLAUDE.md hierarchy: user-level (`~/.claude/CLAUDE.md`) → project-level (`.claude/CLAUDE.md` or root) → directory-level
- `@import` for modular CLAUDE.md files
- `.claude/rules/` with YAML frontmatter `paths` for conditional loading
- Custom commands: `.claude/commands/` (project) vs `~/.claude/commands/` (user)
- Skills: `.claude/skills/` with `SKILL.md`, frontmatter: `context: fork`, `allowed-tools`, `argument-hint`
- Personal skill customization in `~/.claude/skills/`
- `/memory` command to verify which files are loaded
- **Specification Precision lens**: CLAUDE.md is how you "talk English to a machine" at the project level. Vague CLAUDE.md = specification drift. Every instruction must be literal and unambiguous.
- **Specification Drift prevention**: CLAUDE.md forces the agent to re-read specifications at session start. It can't "forget" what it can't miss.

### BUILD (60 min)
- **Module 5**: `claude-code-config` in the skill
  - CLAUDE.md hierarchy visualizer
  - Config generator: "What's your team workflow?" → generates CLAUDE.md + rules
  - Quiz: "A new team member isn't getting project instructions. Why?"
  - Skill template generator with frontmatter options

### TEACH (30 min)
- **Blog post**: "CLAUDE.md is Your AI's Operating System — Here's How to Configure It"
  - Hook: "Your team's AI assistant is only as good as its instructions file"
  - Practical: real CLAUDE.md examples for different team workflows
- **YT short**: "3 CLAUDE.md Mistakes That Cost Your Team Hours"
- **LinkedIn**: Share your CLAUDE.md template

### REFLECT (15 min)
- 5 practice questions on Claude Code configuration
- Update skill with config templates

---

## Day 6 (Apr 3) — PLAN MODE, CI/CD & ITERATION: The Builder's Workflow

**Domain**: D3 — Claude Code Configuration (Task Statements 3.4, 3.5, 3.6)
**Real-World Skill Built**: Decomposing Tasks & Delegating (#3) — plan mode IS task decomposition
**Failure Modes Active**: Cascading Failure (#5) in CI/CD pipelines

### LEARN (45 min)
- Plan mode: for complex tasks (multi-file, architectural decisions)
- Direct execution: for well-scoped changes (single-file bug fix)
- Explore subagent: isolates verbose discovery, preserves main context
- Iterative refinement: input/output examples > prose descriptions
- Test-driven iteration: write tests first, iterate by sharing failures
- CI/CD: `-p` flag for non-interactive, `--output-format json` + `--json-schema`
- Session context isolation: generator shouldn't review its own code (same context = blind spots)
- **Task Decomposition lens**: Plan mode is the "planner agent" pattern applied to code. Claude decomposes the task before executing — this is the correct sequence.
- **Cascading Failure in CI/CD**: When a pipeline stage fails silently, all downstream stages produce garbage. The fix: verification loops between stages, not just at the end.

### BUILD (60 min)
- **Module 6**: `workflow-patterns` in the skill
  - Decision tree: "Plan mode or direct execution?" (interactive)
  - CI/CD pipeline template generator
  - Quiz: "Why shouldn't the same Claude session that wrote code review it?"
  - Iterative refinement demo: few-shot → test → iterate cycle

### TEACH (30 min)
- **Blog post**: "Plan Mode vs. Just Do It: When to Think Before Coding with Claude"
  - Hook: "Asking Claude to 'just fix it' on a 200-file refactor is like asking a surgeon to 'just wing it'"
  - Framework: 1-file = execute, multi-file = plan, unknown scope = explore first
- **YT short**: "The Claude Code Flag That Unlocks CI/CD Magic"
- **X**: CI/CD integration cheat sheet

### REFLECT (15 min)
- 5 practice questions on plan mode and CI/CD
- Full Domain 3 review

---

## Day 7 (Apr 4) — PROMPT ENGINEERING: Making Claude Do What You Mean

**Domain**: D4 — Prompt Engineering & Structured Output (Task Statements 4.1, 4.2, 4.3)
**Real-World Skill Built**: Specification Precision (#1) + Evaluation & Quality Judgment (#2)
**Failure Modes Active**: Specification Drift (#2), Sycophantic Confirmation (#3)

### LEARN (45 min)
- Explicit criteria > vague instructions ("flag when behavior contradicts comments" > "check comments")
- Categorical criteria > confidence-based filtering ("be conservative" doesn't work)
- Few-shot examples: THE technique for consistent output
  - Show reasoning for ambiguous cases
  - Demonstrate desired format (location, issue, severity, fix)
  - Distinguish acceptable patterns from genuine issues
- Structured output via `tool_use` + JSON schemas = guaranteed schema compliance
- `tool_choice`: `"auto"` / `"any"` / forced — know when to use each
- Schema design: required vs optional, enum + "other", nullable for missing info
- **Specification Precision lens**: A vague prompt is a specification failure. The anti-example from job postings: "Come up with a solution because you've read the tickets." That IS the vague prompt anti-pattern.
- **Evaluation lens**: AI is "fluently wrong" — confident AND incorrect. Before shipping any output, ask: is it *functionally* correct (right answer) not just *semantically* correct (sounds right)?
- **Sycophantic Confirmation**: If your few-shot examples contain wrong data, the model will confirm and amplify them. Garbage in, confident garbage out.

### BUILD (60 min)
- **Module 7**: `prompt-patterns` in the skill
  - "Prompt Doctor": paste a vague prompt, get an explicit-criteria rewrite
  - Few-shot example generator for common scenarios
  - JSON schema designer: interactive tool definition builder
  - Quiz: "tool_choice 'any' vs 'auto' — what's the difference?"

### TEACH (30 min)
- **Blog post**: "Few-Shot Examples Are the Only Prompt Technique That Actually Works"
  - Hook: "Detailed instructions are like job descriptions. Few-shot examples are like showing someone how to do the job."
  - Before/after: vague prompt vs few-shot-enhanced prompt
- **YT short**: "The Prompt Trick That 10x'd My Claude Output Quality"
- **LinkedIn**: Share a few-shot template

### REFLECT (15 min)
- 5 practice questions on prompt engineering
- Build 3 few-shot examples for the skill's own prompts

---

## Day 8 (Apr 5) — VALIDATION, BATCH & MULTI-PASS: Production Patterns

**Domain**: D4 — Prompt Engineering (Task Statements 4.4, 4.5, 4.6)
**Real-World Skill Built**: Evaluation & Quality Judgment (#2) + Cost & Token Economics (#7)
**Failure Modes Active**: Silent Failure (#6)

### LEARN (45 min)
- Retry-with-error-feedback: append specific errors to prompt for self-correction
- When retries are useless: info not in source doc vs format errors (retryable)
- `detected_pattern` field for tracking false positive patterns
- Semantic validation errors (values don't sum) vs syntax errors (tool_use eliminates)
- Message Batches API: 50% cost savings, 24-hour window, no multi-turn tool calls
- `custom_id` for correlating batch request/response pairs
- Self-review limitations: same session can't objectively review its own output
- Multi-pass review: per-file local passes + cross-file integration pass
- **Evaluation lens**: "Functional correctness" (actually right) vs "semantic correctness" (sounds right). Silent Failure lives in the gap between these. High-stakes outputs (drug interactions, financial decisions) require functional correctness checks — evaluation harnesses with pass/fail metrics, not vibes.
- **Token Economics lens**: 50% batch savings = real ROI math. Know when to batch (overnight reports, weekly audits) vs when NOT to (pre-merge checks, real-time flows). Multi-agent systems can bleed tokens invisibly — build the spreadsheet before deploying at scale.

### BUILD (60 min)
- **Module 8**: `production-patterns` in the skill
  - Validation flow designer: "What are you extracting?" → retry/validation pipeline
  - Batch vs sync decision tree
  - Quiz: "Your batch job has 500 docs, 23 failed. What's your resubmission strategy?"
  - Multi-pass review architecture diagram generator
  - **Token calculator**: blended cost model across Claude Opus/Sonnet/Haiku for a 100M-token task

### TEACH (30 min)
- **Blog post**: "Why Your AI Can't Catch Its Own Mistakes (And What to Do Instead)"
  - Hook: "Asking Claude to review code it just wrote is like asking a student to grade their own test"
  - Solution: independent review instances, multi-pass architecture
- **YT short**: "The Batch API Trick That Cuts Your Claude Bill in Half"
- **X thread**: Production validation patterns cheat sheet

### REFLECT (15 min)
- 5 practice questions on validation and batch processing
- Full Domain 4 review
- Calculate: if you run your day's build output through batch instead of sync, what's the cost difference?

---

## Day 9 (Apr 6) — CONTEXT MANAGEMENT & RELIABILITY: The Architect's Final Layer

**Domain**: D5 — Context Management & Reliability (Task Statements 5.1-5.5)
**Real-World Skill Built**: Context Architecture (#6) + Failure Pattern Recognition (#4)
**Failure Modes Active**: ALL SIX — this day is the master review

### LEARN (45 min)

#### Context Window Management (TS 5.1)
- Context window = working memory. It fills up and degrades performance (**Context Degradation**)
- Strategies:
  - Summarization: compress long conversations before they fill the window
  - Selective retention: keep only what's needed for the *current* task
  - External storage + retrieval: don't store everything in context, store a pointer
  - Context budget: allocate tokens explicitly (system prompt budget vs example budget vs content budget)

#### Persistent vs Per-Session Context (TS 5.2) — The Librarian Skill
- **Persistent context**: always available, foundational — think Dewey decimal system. Company knowledge base, product catalog, policy docs. Organized so the agent can search and find the "right book."
- **Per-session context**: temporary, task-specific — the "right book" pulled for this one run. Order details, user session data, current task state.
- **Why this matters**: dirty persistent context (unorganized, outdated) = wrong book retrieved = **Silent Failure**. Clean boundaries prevent the agent from "polluting" one session's data into the next.
- Anti-pattern: letting per-session data bleed back into persistent context

#### Multi-Agent Context Handoffs (TS 5.3)
- Subagents have isolated context — coordinator must explicitly pass needed information
- Handoff protocol: structured summary of state, not raw conversation history
- What to include: customer ID, resolved steps, current issue, recommended next action
- What NOT to include: the full conversation dump (wastes tokens, causes Context Degradation downstream)

#### Human-in-the-Loop & Escalation (TS 5.4)
- Escalation triggers: sentiment score threshold, repeated failure, ambiguous authorization request
- Graceful degradation: if a tool fails, what's the fallback? Define it at design time.
- Human-in-the-loop checkpoints: before irreversible actions, before high-stakes outputs
- Self-evaluation: Claude checking its own work in an *independent* session (not same context)

#### Reliability Patterns (TS 5.5)
- Retry with backoff for transient failures
- Fallback tool chains: primary tool fails → secondary tool → human escalation
- Verification loops: don't trust any agent output before the next stage acts on it
- **Specification Drift prevention**: periodically re-inject original goals into long-running sessions ("forcibly remind")

#### 6 Failure Mode Master Review
Go through each failure mode and name the specific D5 mechanism that prevents it:
- Context Degradation → context budget + summarization
- Specification Drift → goal re-injection + CLAUDE.md
- Sycophantic Confirmation → independent validation + functional correctness checks
- Tool Selection Errors → tool design (Day 3) + 4-5 tool limit
- Cascading Failure → coordinator pattern + verification loops
- Silent Failure → evaluation harnesses + multi-pass review + functional correctness test

### BUILD (60 min)
- **Module 9**: `context-reliability` in the skill
  - Context budget calculator: "How many tokens for system prompt vs examples vs content?"
  - Persistent vs per-session context classifier (interactive)
  - Escalation decision tree builder
  - Full failure mode diagnostic tool: "Describe the symptom → get the failure mode + fix"
  - Quiz: full Domain 5 question bank (8 questions, hardest day)

### TEACH (30 min)
- **Blog post**: "The 6 Ways AI Agents Fail (And How to Design Against Each One)"
  - Hook: "There are exactly 6 ways your AI agent will break in production. Here they are."
  - Cover all 6 failure modes with one concrete example each
  - End with: "The last one — Silent Failure — is why you need evaluation harnesses, not vibes"
- **YT short**: "Persistent vs Per-Session Context: The Distinction That Scales AI"
- **LinkedIn**: The 6 failure modes as a shareable visual (you built this knowledge, share it)

### REFLECT (15 min)
- Full Domain 5 review
- Cross-domain review: weakest areas from Days 1-9
- Update all skill modules with cross-references
- **The 7 skills check**: which of the 7 core AI professional skills do you feel strongest in? Weakest? Bring that answer to Day 10.

---

## Day 10 (Apr 7) — EXAM DAY: Ship, Test, Certify

### MORNING: Final Prep (60 min)
- Full scenario practice: simulate all 6 exam scenarios
- Use the skill's quiz mode for rapid-fire questions across all domains
- Focus on weakest domain from Day 9 reflection
- Review all 5 domain summaries
- **Last check**: For each of the 7 professional skills, name one exam question type that tests it

### MIDDAY: Graduate Skill → Plugin (60 min)
- Package `.claude/skills/claude-architect-mastery/` into a Cowork `.plugin` file
- Write plugin README and marketplace description
- Push final version to GitHub
- Record capstone YouTube video: "How I Passed Claude Architect in 10 Days"

### AFTERNOON: Take the Exam
- Take the exam
- Target: 850+/1000
- Post results to LinkedIn, X, newsletter

### EVENING: Publish & Launch
- Publish the plugin
- Write launch blog post: "I Built a Plugin While Studying for the Cert — Here's Both"
- Share across all channels

---

## Mastery Tracker

| Domain | Weight | Day(s) | Core Skill(s) | Confidence (1-5) | Notes |
|--------|--------|--------|----------------|-------------------|-------|
| D1: Agentic Architecture | 27% | 1, 2, 4 | #3 Decompose, #4 Failure Patterns, #5 Trust | ☐ | |
| D2: Tool Design & MCP | 18% | 3 | #5 Trust & Security | ☐ | |
| D3: Claude Code Config | 20% | 5, 6 | #1 Spec Precision, #3 Decompose | ☐ | |
| D4: Prompt Engineering | 20% | 7, 8 | #1 Spec Precision, #2 Evaluation, #7 Token Cost | ☐ | |
| D5: Context & Reliability | 15% | 9 | #6 Context Architecture, #4 Failure Patterns | ☐ | |

Update confidence daily. Any domain below 4 by Day 9 gets extra time on Day 10.

---

## The Flywheel Math

By Day 10, you will have:
- **10 blog posts** → 10 LinkedIn posts, 10 YouTube scripts, 10 X threads
- **9 skill modules** → 1 polished plugin
- **~50 practice questions** created (teaching is the deepest learning)
- **1 certification** (Claude Certified Architect — Foundations)
- **1 launchable product**
- **7 professional skills** built and named — not just cert prep, career capital
- **1 content brand milestone** (10-day streak on love12xfuture)

Each output feeds the others. The plugin teaches what you learned. The content markets the plugin. The certification validates both. The 7 skills make you dangerous in any AI room.

This is Isaiah 35: the barren 10 days bloom into a certification, a product, a content library, a practitioner's framework, and a business seed.
