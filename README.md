# OmegaFounders — Silicon Valley AI Cohort

**The AI job market is splitting in two.**
One side has 3.2 open roles per qualified candidate. The other side is getting automated.

OmegaFounders is a 12-day Silicon Valley cohort for founders and engineers who want to land on the right side of that split — by shipping real AI systems, earning the Claude Certified Architect credential, and demoing in front of investors.

> "The desert shall rejoice and blossom." — Isaiah 35

---

## What This Repo Is

| Path | What's in it |
|---|---|
| [`landing/`](./landing/) | Landing pages — dark premium & Golden Hour variants |
| [`docs/10-DAY-PLAN.md`](./docs/10-DAY-PLAN.md) | Full 12-day sprint plan (days 1–10 learn/build/post, 11 demo, 12 graduation) |
| [`docs/STUDY-RESOURCES.md`](./docs/STUDY-RESOURCES.md) | Certification prep resources |
| [`skills/claude-architect-mastery/SKILL.md`](./skills/claude-architect-mastery/SKILL.md) | AI-powered exam prep skill (works inside Claude Desktop) |
| [`learn-build-teach/`](./learn-build-teach/) | Daily public learning logs — every cohort member's daily entry |
| [`.github/`](./.github/) | PR templates, issue templates, CI workflows |

---

## Why Join

<details>
<summary><strong>📈 The K-Shaped Market — Why timing matters right now</strong></summary>

**Real-life benefit**: The AI job market is not rising uniformly. It is splitting into a K — top track accelerates, bottom track contracts. The inflection point is whether you can *build and evaluate* AI systems, not just use them. The K-split creates 3.2 open roles for every qualified AI architect candidate, right now, in 2026.

**The innovation**: Most AI "education" teaches prompt tricks. OmegaFounders teaches the 7 core professional skills that show up in every job description for AI Architect, AI Product Lead, and AI Engineer roles: Specification Precision, Evaluation & Quality Judgment, Task Decomposition, Failure Pattern Recognition, Trust & Security Design, Context Architecture, and Cost & Token Economics.

**How it works**: The 12-day sprint is structured as a Learn → Build → Teach flywheel. Each day maps to a specific certification domain, produces a shipped artifact, and requires a public post. By Day 12 you have a portfolio, a credential, and a demo — not just course completion.

[📋 See the full 12-day plan →](./docs/10-DAY-PLAN.md)

</details>

<details>
<summary><strong>🤖 Claude Architect Mastery Skill — AI that teaches you AI</strong></summary>

**Real-life benefit**: Exam prep is usually passive (read → forget). This skill is active: it quizzes you in exam format, simulates the 6 real scenarios, tracks your mastery across domains, and coaches your weakest areas. It turns any Claude Desktop session into a personalized certification tutor.

**The innovation**: The skill encodes practitioner-level understanding, not just exam trivia. It knows the *why* behind every pattern — why hooks beat prompts for compliance, why 4–5 tools is optimal per agent, why the same model can't review its own output. It also drills the 6 AI failure modes that separate architects from prompt engineers.

**How it works**: Drop `skills/claude-architect-mastery/SKILL.md` into `.claude/skills/` in your Claude Desktop workspace. Then invoke it with: `quiz me on Domain 1`, `simulate Scenario 3`, `explain failure modes`, `show my mastery`, or `Day 4 study session`. It runs in a forked context so it doesn't pollute your main conversation.

[🔧 View the skill →](./skills/claude-architect-mastery/SKILL.md)

</details>

<details>
<summary><strong>🏗️ 12-Day Build Structure — Ship every day, not just at the end</strong></summary>

**Real-life benefit**: Most bootcamps front-load learning and back-load building. You come out with knowledge but no portfolio. OmegaFounders structures every day as: 45 min Learn → 90 min Build → 30 min Teach (public post). By Day 10 you have 10 shipped artifacts and 10 public posts. By Day 12 you have a demo and a credential.

**The innovation**: The 10-day learning track is mapped 1:1 to the Claude Certified Architect exam domains, weighted by exam percentage (D1: 27%, D3: 20%, D4: 20%, D2: 18%, D5: 15%). Day 11 is Founder Demo Day. Day 12 is Invited Speakers + Graduation. The structure forces compound learning — each day builds on the last.

**How it works**: Each day in `docs/10-DAY-PLAN.md` has: task statement numbers, real-world skill built, active failure modes to recognize, and a concrete BUILD artifact. The plan integrates the 7 core professional skills and 6 failure modes across all 10 learning days — not as separate chapters, but as the operating system for each day's work.

[📅 Full sprint plan →](./docs/10-DAY-PLAN.md)

</details>

<details>
<summary><strong>🌐 Landing Pages — Two variants, zero JavaScript frameworks</strong></summary>

**Real-life benefit**: The landing page converts visitors into cohort applicants. Two design variants let you A/B test conversion: dark premium (elite Silicon Valley vibe) vs. Golden Hour (warm community vibe). Both are single-file HTML — no build step, no npm, deployable anywhere.

**The innovation**: Both pages use Apple-grade micro-interactions: `backdrop-filter: saturate(180%) blur(20px)` frosted glass nav, `meshDrift` animated gradient mesh, `fadeSlideUp` scroll reveals via IntersectionObserver, and a CSS-only ticker tape. The dark version adds a live K-shaped market visualization with the 7 skills grid. Neither page has a framework dependency.

**How it works**: Open `landing/index.html` or `landing/golden-hour.html` in any browser. No build step. Drop either onto any static host (Netlify, Vercel, GitHub Pages, Cloudflare Pages) and it works. The K-shaped market section is interactive and data-driven from CSS variables — change `--gold` in 1 line to rebrand the entire page.

[🌑 Dark version →](./landing/index.html) · [🌅 Golden Hour →](./landing/golden-hour.html)

</details>

<details>
<summary><strong>🤝 Multi-Contributor Flywheel — Each cohort member's daily log lives here</strong></summary>

**Real-life benefit**: Public learning creates accountability *and* audience. When 20 founders are each posting daily logs to this repo, the repo itself becomes a content marketing machine. Each PR is a signal of activity; each daily log is a tweet thread waiting to happen.

**The innovation**: The `learn-build-teach/` directory has a standard daily log template. Each contributor creates `learn-build-teach/[github-handle]/day-[N].md`. The PR template enforces the structure: what you learned, what you built, where you posted. This is a living portfolio, not a graveyard of completed coursework.

**How it works**: Fork the repo → create your folder → submit a PR per day using the daily log template. CI validates your markdown has the required sections. See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full workflow.

[📝 Daily log template →](./learn-build-teach/TEMPLATE.md) · [🤝 Contributor guide →](./CONTRIBUTING.md)

</details>

---

## Quick Start (Cohort Member)

```bash
# 1. Fork this repo to your account
# 2. Clone your fork
git clone https://github.com/YOUR_HANDLE/omega-founders.git
cd omega-founders

# 3. Set up the exam prep skill
cp skills/claude-architect-mastery/SKILL.md ~/.claude/skills/

# 4. Create your daily log folder
mkdir -p learn-build-teach/YOUR_HANDLE
cp learn-build-teach/TEMPLATE.md learn-build-teach/YOUR_HANDLE/day-01.md

# 5. Read the 12-day plan
open docs/10-DAY-PLAN.md

# 6. Start Day 1
# Then submit a PR with your day-01.md at end of day
```

---

## Quick Start (Contributor / Builder)

```bash
# Clone the main repo
git clone https://github.com/wjlgatech/omega-founders.git
cd omega-founders

# Create a feature branch (one branch per PR)
git checkout -b feat/your-feature-name

# Make your changes
# Then open a PR using the PR template
```

See [CONTRIBUTING.md](./CONTRIBUTING.md) for branch naming, commit conventions, and the PR checklist.

---

## Cohort Schedule

| Day | Focus | Domain |
|---|---|---|
| 1 | Agentic Loops | D1 (27%) |
| 2 | Multi-Agent Architecture | D1 |
| 3 | Tool Design | D2 (18%) |
| 4 | MCP Integration | D2 |
| 5 | Claude Code + CLAUDE.md | D3 (20%) |
| 6 | Workflows & CI/CD | D3 |
| 7 | Prompt Engineering | D4 (20%) |
| 8 | Structured Output & Batch | D4 |
| 9 | Context & Reliability | D5 (15%) |
| 10 | Exam + Final Ship | All |
| 11 | **Founder Demo Day** 🎯 | Ship |
| 12 | **Invited Speakers + Graduation** 🏆 | Celebrate |

---

## The 7 Core AI Professional Skills

Every day in the cohort develops at least one of these:

| # | Skill | Exam Domain |
|---|---|---|
| 1 | Specification Precision | D4 |
| 2 | Evaluation & Quality Judgment | D4, D5 |
| 3 | Task Decomposition & Delegation | D1 |
| 4 | Failure Pattern Recognition | D5, D1 |
| 5 | Trust & Security Design | D1, D2 |
| 6 | Context Architecture | D5, D3 |
| 7 | Cost & Token Economics | D4, D5 |

---

## The 6 AI Failure Modes

Know these cold. They show up in the exam and in every production system:

| # | Failure Mode | Why It Matters |
|---|---|---|
| 1 | Context Degradation | Quality drops silently as conversation grows |
| 2 | Specification Drift | Output drifts from intent across agent handoffs |
| 3 | Sycophantic Confirmation | Model agrees with wrong premises |
| 4 | Tool Selection Errors | Wrong tool called, or no tool when needed |
| 5 | Cascading Failure | Error amplifies through multi-agent pipeline |
| 6 | **Silent Failure** ⚠️ | System "succeeds" but produces wrong results |

[Full deep-dive on all 6 →](./skills/claude-architect-mastery/SKILL.md)

---

## Star History

If this repo is helping you — star it. Every star is a vote that the right side of the K-split should be accessible, not gatekept.

---

## License

MIT. Build on it, ship with it, teach from it.

---

*Built during a 12-day sprint. Powered by stubbornness, Claude, and Isaiah 35.*
