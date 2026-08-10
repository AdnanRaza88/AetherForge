# AetherForge — Autonomous Hierarchical Coding Agent System
## Overview, Idea Validation & Gap Analysis

**Codename:** AetherForge  
**Version:** 0.1 (Planning Phase)  
**Date:** 2026-08-10  
**Status:** Spec-Driven Planning Complete — Implementation Not Started

---

## 1. Core Vision

A personal autonomous software engineering organization that lives on the user’s devices and GitHub.

- One **Main Agent** acts as the Software Engineering Lead / CTO-level orchestrator.
- 10–50 specialized **Sub-Agents** (employees) handle focused work: backend, frontend, data science / ML / DL, testing, DevOps, security, documentation, research, etc.
- Everything is driven by **Spec-Driven Development** (Spec → Design → Tasks → Implementation → Verification).
- Fully model-agnostic: any cloud API (OpenAI, Anthropic, xAI, Google, etc.) + fully local open-source models (Ollama, llama.cpp, vLLM, LM Studio, etc.).
- Cross-device control: Desktop (Windows/Mac/Linux), Laptop, Android, iPhone. Mobile is primarily a control & approval surface; heavy execution happens on desktop or remote sandboxes.
- ChatGPT/Claude/Grok-style conversation UI with full history, resume, new chat.
- **Permission-first**: every non-trivial action requires explicit user approval (or scoped pre-approved policies).
- Native **Voice Mode**: talk as if on a phone call; agent executes while conversing.
- User-configurable: GitHub token, device permissions, API keys, custom skills/instructions, agent roles.

---

## 2. Idea Validation (2026 Landscape)

### Strengths (Aligned with Current Best Practices)
- Hierarchical multi-agent systems are proven (CrewAI hierarchical mode, MetaGPT company simulation, OpenHands multi-agent, recent hierarchical SWE agents on SWE-bench).
- Spec-Driven Development is the dominant high-quality pattern (GitHub Spec Kit, AWS Kiro, Tessl, Akka Specify). “Vibe coding” is declining for serious work.
- Model-agnostic + local-first is table stakes (Aider, Continue, Goose, OpenHands, OpenCode, Cline all support this).
- GitHub as primary workspace + PR/issue/repo operations is the natural home for coding agents.
- Voice + always-available control plane is emerging (OpenClaw, LiveKit Agents, Pipecat).
- Permission / human-in-the-loop gates are required for any system that can delete repos or modify production code.

### Competitive Positioning
- More hierarchical and “company-like” than single-agent tools (Aider, Cline, Goose).
- More personal and local-first than pure cloud agents (Devin).
- Stronger Spec-Driven process than most current agents.
- Cross-device + voice is a differentiator for personal use.

### Realistic Scope Assessment
High ambition. Success depends on phased delivery and clear boundaries (especially mobile).

---

## 3. Critical Gaps & Risks Identified

| Area | Gap / Risk | Severity | Recommendation |
|------|------------|----------|----------------|
| Mobile Execution | Full coding agent on Android/iOS is severely limited by OS sandbox, no reliable long-running terminal, battery, background restrictions | Critical | Mobile = Control Plane only (chat, voice, approve/reject, view status, lightweight file ops). Heavy work on Desktop or Remote Sandbox (E2B / Docker / own server). |
| Security Model | GitHub token + filesystem + autonomous actions = high blast radius | Critical | Strict capability-based permissions, action classification (read / write / destructive), mandatory approval for high-risk, full audit log, sandbox by default. |
| Agent Scale | 10–50 sub-agents from day 1 is operationally heavy | High | Start with 5–8 core specialists + dynamic spawning. Hierarchical manager → specialists. |
| Runtime Environment | Where does code actually run and get tested? | High | Primary: local Docker sandbox (OpenHands style). Secondary: remote sandboxes. Device-native only for lightweight tasks. |
| Cross-Device State | Conversations, agent memory, project context must sync | High | Local-first with encrypted sync (or user-controlled backend). Conversation history + vector memory + project workspace state. |
| Cost & Rate Limits | Multiple parallel agents can explode cost | Medium | Local model preference, budget caps, agent priority queues, fallback to cheaper/local models. |
| Observability | Hard to debug multi-agent failures | Medium | Full action traces, decision logs, cost per task, success/failure metrics. |
| Skills System | Custom instructions/skills mentioned but not detailed | Medium | Skill files (Markdown + optional code) that can be attached per agent or globally (inspired by OpenClaw / AGENTS.md). |
| Offline Capability | Local models help, but GitHub ops need network | Low-Medium | Graceful degradation: local planning + queue of GitHub actions when online. |

---

## 4. Recommended Core Architecture Principles

1. **Spec is Source of Truth** — Nothing is implemented without a reviewed Spec → Design → Tasks chain.
2. **Main Agent = Orchestrator only** — It plans, decomposes, assigns, reviews, and reports. It rarely writes large amounts of code itself.
3. **Sub-Agents are Specialists** — Narrow context, specialized tools and system prompts.
4. **Permission Gate is Non-Negotiable** — Configurable policies + per-action approval for anything beyond read/low-risk.
5. **Local-First, Cloud-Optional** — Prefer local models and local sandboxes. Cloud APIs and remote sandboxes are accelerators.
6. **Mobile is Remote Control** — Not the primary execution environment.
7. **Everything Auditable** — Every plan, tool call, file change, and GitHub action is logged and reviewable.

---

## 5. Document Set Produced

| File | Purpose |
|------|---------|
| 00-overview-and-analysis.md | This document |
| 01-PRD.md | Product Requirements Document |
| 02-system-design.md | High-level architecture & component design |
| 03-technical-spec.md | Detailed technical specification |
| 04-schemas.md | Data models, agent schemas, API contracts |
| 05-agents-instructions.md | System prompts & behavioral specs for Main + Sub-Agents |
| 06-tasks-and-roadmap.md | Phased task breakdown & milestones |
| 07-tracker.md | Living project tracker (status, risks, decisions) |
| 08-security-privacy.md | Security model, threat model, privacy |
| 09-ux-frontend-spec.md | Conversation UI, settings, voice, mobile control plane |
| 10-source-repositories.md | Curated inspiration repositories |

---

## 6. Next Steps After Planning Review

1. User reviews and gives feedback on all documents.
2. Lock product decisions (especially mobile scope and permission model).
3. Choose initial tech stack (proposed in Design & Spec).
4. Begin Phase 0: Core runtime + Main Agent + single Sub-Agent proof-of-concept.

---

**This is a company-in-a-box for software engineering, controlled by the user, running primarily on their own hardware, with GitHub as the shared workspace.**
