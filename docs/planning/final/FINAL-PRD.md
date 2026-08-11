# AetherForge — Final Product Requirements Document (PRD)
**Version:** 1.0 Final  
**Status:** Locked for Implementation  
**Date:** 2026-08-11  

---

## 1. Vision

AetherForge is a personal autonomous engineering organization that runs on the user’s devices.  
One Main Orchestrator leads multiple **Domain Armies** (Game, Backend, Frontend, ML, DL, Data Science, Content, Security, etc.).  
Every significant piece of work starts with rigorous Spec-Driven planning so that all agents share the same truth about theme, style, architecture, and constraints.  
The system is model-agnostic, permission-first, local-first where possible, and works across Desktop, Mac, Android, and iPhone (control plane on mobile).

---

## 2. Core Goals (Locked)

1. Spec-Driven Development is mandatory for non-trivial work.
2. Hierarchical multi-agent system with Domain Sections / Armies.
3. Strong Shared Context protocol so parallel agents never drift on theme, colors, style, or architecture.
4. Dynamic Skills loaded on-demand from a web Skills Registry (no heavy local installs).
5. Large-codebase review and fix protocol that avoids context explosion and cascading new bugs.
6. Model-agnostic (any cloud API + local open-source models).
7. Python-based agent runtime for speed and ecosystem strength.
8. Web-based cross-platform UI (Tauri + PWA) with liquid-glass / glassmorphism design.
9. Permission-first: every non-trivial action requires approval or explicit policy.
10. Voice Mode and full conversation history/resume.

---

## 3. Platform & Runtime Decisions (Locked)

| Decision | Choice | Reason |
|----------|--------|--------|
| Agent Runtime Language | Python | Fast enough with async, richest agent + AI ecosystem, user preference |
| UI Stack | Tauri (desktop) + PWA (mobile) | Cross-platform, native feel, single web codebase |
| Code Execution | Local Docker sandbox (primary) + Remote sandbox option + Local server | Open-source, works offline when possible |
| Mobile Role | Control Plane (chat, voice, approve, status) | Realistic given OS limits |
| Desktop / Mac | Full execution + control | Primary power surface |
| Skills | Dynamic fetch from web Skills Registry | Zero install, always up-to-date, lightweight |

---

## 4. Domain Armies / Sections

The system maintains specialized **Sections**. The Main Orchestrator routes the user query to the correct Section Lead(s).

Examples of Sections (extensible):
- Game Development Army (full stack of game specialists)
- Backend Army
- Frontend Army
- Machine Learning Army
- Deep Learning Army
- Data Science Army
- Content / Video / Documentation Army
- Security & Review Army
- DevOps / Infrastructure Army
- Research & Exploration Army

Each Section has:
- Section Lead (mid-level orchestrator)
- Specialist Sub-Agents
- Section-specific Skills (loaded on demand)
- Shared Context injection rules

---

## 5. Critical Problems Solved by Design

### 5.1 Consistency Drift (Theme / Color / Style mismatch)
**Problem:** Parallel agents produce inconsistent results (dark night sky + sniper that cannot see).  
**Solution:** Mandatory Shared Project Context Document created in the Planning phase. Every agent receives the same Theme Guide, Color Palette, Architecture Decisions, and Constraints before any code is written. Parallel work only starts after this document is approved.

### 5.2 Large Code Review & Safe Fixing
**Problem:** AI is good at finding bugs but when fixing, it creates many new bugs because of context overflow.  
**Solution:** Structured multi-agent review protocol:
1. Structure Explorer maps files and ownership.
2. Code is partitioned.
3. Parallel specialized Reviewers work on partitions and produce summaries only.
4. Aggregator compiles clean summaries.
5. Fixes are applied in small verified chunks with re-review.
6. Full context is never dumped into one agent.

---

## 6. Spec-Driven Workflow (Enforced)

Every non-trivial request follows:

1. **Clarify** (if needed)
2. **Specify** → Requirements + Acceptance Criteria
3. **Design** → Architecture + Shared Project Context (theme, colors, constraints)
4. **Tasks** → Ordered, reviewable tasks with clear owners
5. **Implement** (only after approval)
6. **Verify** against acceptance criteria
7. **Integrate** (PR etc.)

The system itself produces the planning documents before any specialist coding begins.

---

## 7. Skills System

- Central Skills Registry (web URL configured by user or default).
- Agents fetch only the skill files they need at runtime.
- Skills are lightweight Markdown + optional tool definitions.
- Category-specific planning templates live in skills.
- No global heavy installation required.

---

## 8. Non-Functional Requirements

- Fast and smooth (async Python, efficient context management, streaming UI).
- Local-first data and models when possible.
- Full audit trail.
- Zero unauthorized side effects.
- Clean, human-written code style (no AI filler, no unnecessary comments).

---

## 9. Success Criteria for v1

- User can request a game / full-stack feature / ML pipeline and receive consistent, themed results from parallel agents.
- Large repository review produces useful summaries and safe incremental fixes.
- System feels responsive on modern hardware.
- Skills load dynamically without friction.
- Single CLAUDE.md file lets any coding agent understand the entire project instantly.

---

**This PRD is the source of truth for product scope.**  
All other documents derive from it.
