# Day 1 — wjlgatech

**Date**: 2026-03-31
**Domain**: D1 — Agentic Architecture & Orchestration (Task Statement 1.1)
**Cohort Day**: 1 of 12

---

## What I Learned

- `stop_reason: "tool_use"` → continue the loop. `stop_reason: "end_turn"` → terminate. Checking text content instead of `stop_reason` is an explicit anti-pattern on the exam.
- Tool results **must** be appended to conversation history before the next LLM call — this is structural, not optional. Without this, the loop has no memory of what it just did.
- 3 anti-patterns to burn in: (1) parsing NL for termination signal, (2) arbitrary iteration cap as primary stop, (3) checking assistant text content instead of `stop_reason`.
- Model-driven decision trees decide dynamically at runtime. Pre-configured trees are hardcoded branching. The exam distinguishes these for adaptability vs predictability tradeoffs.

---

## What I Built

**Artifact**: Agentic loop reference diagram (ASCII) — canonical lifecycle from initial call through tool execution to `end_turn`

**Link**: See Day 1 section in [docs/10-DAY-PLAN.md](../../docs/10-DAY-PLAN.md)

---

## Where I Posted

**Platform**: LinkedIn
**Link**: _(to be added after posting)_

---

## Skill Built Today

**Primary skill**: #1 — Specification Precision
**How**: Wrote an agentic loop implementation spec with explicit stop conditions — not "when done" but "when `stop_reason == 'end_turn'`"

---

## Failure Mode Spotted

**Failure mode**: #4 — Tool Selection Error (adjacent to today's domain)
**What happened**: While building the loop, noticed that `tool_choice: "auto"` allows text response instead of tool call — which silently breaks the loop if the model decides not to call a tool.
**How I handled it**: Documented: use `tool_choice: "any"` when a tool call is required for the loop to proceed.

---

## Tomorrow

**Day 2 domain**: D1 — Multi-Agent Architecture (TS 1.2–1.4): hub-and-spoke, isolated subagent context, coordinator patterns
**Planned build artifact**: Multi-agent diagram + coordinator system prompt template
