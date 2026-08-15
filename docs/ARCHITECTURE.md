# AetherForge — System Architecture
**Version:** 1.0  
**Status:** Final  
**Last Updated:** 2026-08-15

---

## 1. Purpose of This Document

This is the definitive architecture reference for AetherForge.  
It describes how the system is structured, how components interact, how data flows, and the key design decisions that make consistency, safe large-scale work, and multi-platform operation possible.

Read this after `CLAUDE.md`.

---

## 2. Design Goals

| Goal | How Architecture Supports It |
|------|------------------------------|
| Consistency across parallel agents | Mandatory Shared Project Context injected into every specialist |
| Safe large-codebase review & fix | Partitioned review + summary aggregation + small verified fixes |
| Domain specialization | Hierarchical Domain Armies with Section Leads |
| Model agnosticism | Model Router abstraction |
| Cross-platform | Tauri (desktop) + PWA (mobile) single web codebase |
| Speed & smoothness | Async Python, aggressive context management, streaming |
| Permission safety | Central Permission Gateway in front of every side-effect |
| Extensibility | Dynamic Skills Registry + configurable Sections |

---

## 3. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              USER SURFACES                                   │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────────────┐  │
│  │  Desktop / Mac   │  │  Android / iOS   │  │  Voice Channel            │  │
│  │  (Tauri App)     │  │  (PWA)           │  │  (STT / TTS)              │  │
│  │  Full Power      │  │  Control Plane   │  │                           │  │
│  └────────┬─────────┘  └────────┬─────────┘  └────────────┬──────────────┘  │
└───────────┼─────────────────────┼─────────────────────────┼─────────────────┘
            │                     │                         │
            └─────────────────────┼─────────────────────────┘
                                  │
                    ┌─────────────▼─────────────┐
                    │      CONTROL PLANE         │
                    │  • Conversation Manager    │
                    │  • Session & Project State │
                    │  • Permission Gateway      │
                    │  • Voice Gateway           │
                    │  • Settings & Secrets      │
                    └─────────────┬─────────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
    ┌─────────▼─────────┐ ┌───────▼───────┐ ┌─────────▼─────────┐
    │  MAIN ORCHESTRATOR │ │ MODEL ROUTER  │ │  MEMORY & STATE   │
    │  (Lead Agent)      │ │ (Any LLM)     │ │  (SQLite + Files) │
    └─────────┬─────────┘ └───────────────┘ └───────────────────┘
              │
              │ classifies & routes
              ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         DOMAIN SECTIONS / ARMIES                             │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│  │ Game Army   │ │ Backend     │ │ Frontend    │ │ ML / DL / DS│   ...      │
│  │ Section Lead│ │ Section Lead│ │ Section Lead│ │ Section Lead│            │
│  │ + Specs     │ │ + Specs     │ │ + Specs     │ │ + Specs     │            │
│  └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘            │
└─────────┼───────────────┼───────────────┼───────────────┼───────────────────┘
          │               │               │               │
          └───────────────┴───────────────┴───────────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
    ┌─────────▼─────────┐ ┌───────▼───────┐ ┌─────────▼─────────┐
    │    TOOL LAYER     │ │   SANDBOX     │ │  SKILLS REGISTRY  │
    │  File • Shell     │ │   RUNTIME     │ │  (Web Fetch)      │
    │  Git • GitHub     │ │ Docker /      │ │                   │
    │  Browser • Search │ │ Remote        │ │                   │
    └───────────────────┘ └───────────────┘ └───────────────────┘
```

---

## 4. Component Breakdown

### 4.1 Control Plane
Responsible for all user interaction and safety gates.

- **Conversation Manager**: Threads, history, resume, streaming.
- **Permission Gateway**: Classifies every action (Read / Low-Risk Write / High-Risk / Destructive). Enforces policy or asks user.
- **Session & Project Manager**: Tracks active Shared Context, open tasks, running agents.
- **Voice Gateway**: Real-time STT/TTS with barge-in support.
- **Settings & Secrets Store**: Encrypted storage for tokens, API keys, policies.

### 4.2 Main Orchestrator
The single brain that never writes large amounts of production code itself.

Responsibilities:
1. Understand user goal.
2. Drive Spec → Design → Shared Project Context → Tasks.
3. Get user approval on Shared Context.
4. Route work to the correct Domain Section(s).
5. Monitor progress and handle failures.
6. Aggregate final results and present to user.

### 4.3 Domain Sections (Armies)
Each Section is a mini-organization:

- **Section Lead**: Mid-level orchestrator for that domain.
- **Specialist Sub-Agents**: Narrow, focused workers.
- **Section Skills**: Loaded on demand from the Skills Registry.

Examples: Game Army, Backend Army, Frontend Army, ML Army, DL Army, Data Science Army, Content Army, Security & Review Army, DevOps Army, Research Army.

### 4.4 Model Router
Unified interface for every LLM call.

- Supports any OpenAI-compatible endpoint + native local (Ollama, LM Studio, etc.).
- Fallback chains, cost tracking, per-role model preferences.
- Streaming support.

### 4.5 Tool Layer
All side-effecting capabilities:

- Scoped filesystem
- Sandboxed shell
- Git operations
- GitHub API (create/delete repo, PR, issues, etc.)
- Web search / browse (optional)
- Code execution & test runners

Every tool call that mutates state goes through the Permission Gateway.

### 4.6 Sandbox Runtime
- Primary: Local Docker containers (isolated, resource-limited).
- Secondary: Remote sandboxes (user-provided or integrated).
- Clean workspace per significant task.
- Ability to install dependencies, run tests, start services.

### 4.7 Skills Registry
- Configurable web URL.
- Skills are lightweight Markdown files (optional front-matter).
- Agents fetch only the skills they need at runtime.
- No local package installation required.

### 4.8 Memory & State
- Conversation history (per thread)
- Project artifacts (Specs, Designs, Shared Context, Task lists)
- Agent working memory (short-lived)
- Summaries from large reviews (preferred over raw code)
- Local SQLite + filesystem. Optional encrypted sync later.

---

## 5. Critical Data Flows

### 5.1 Normal Feature Development Flow

```
User Goal
   ↓
Main Orchestrator → Clarify (if needed)
   ↓
Produce Spec
   ↓
Produce Design + Shared Project Context
   ↓
User Approves Shared Context
   ↓
Break into Tasks
   ↓
Route to Domain Section(s)
   ↓
Section Lead assigns Specialists (all receive Shared Context)
   ↓
Parallel or sequential work inside sandbox
   ↓
Results + summaries back to Section Lead → Main Orchestrator
   ↓
Verification against acceptance criteria
   ↓
Final PR / summary to user
```

### 5.2 Large Codebase Review Flow

```
User: "Review this big repo and find bugs"
   ↓
Structure Explorer → structure summary only
   ↓
Partitioner → coherent partitions
   ↓
N Parallel Specialized Reviewers (each gets only its partition + Shared Context)
   ↓
Each outputs: findings + short summary (never full code)
   ↓
Summary Aggregator → clean prioritized report
   ↓
(Optional) Safe Fixer applies one small change at a time + targeted re-review
```

### 5.3 Skill Loading Flow

```
Agent needs skill "game-art-direction"
   ↓
Skill Loader → HTTP GET from Skills Registry
   ↓
Inject skill content into agent context
   ↓
Agent continues with new knowledge
```

---

## 6. Shared Project Context (Consistency Backbone)

Before any parallel specialist work is allowed, this document must exist and be approved:

- Theme & Mood
- Exact Color Palette (hex + usage rules)
- Typography & spacing rules
- Architecture decisions & naming conventions
- Constraints & non-negotiables
- Glossary
- Acceptance criteria snapshot

Every specialist receives it. No agent may invent theme, colors, or architectural style.

This is the primary mechanism that prevents the “dark sky + blind sniper” class of failures.

---

## 7. Technology Stack (Locked)

| Layer | Choice | Notes |
|-------|--------|-------|
| Agent Runtime | Python (async) | Fast, rich ecosystem |
| UI | Tauri + PWA | Single web codebase, native desktop feel |
| UI Style | Liquid Glass / Glassmorphism | Light theme, frosted panels, restrained |
| Sandbox | Docker (primary) | Isolated, reproducible |
| LLM Interface | Model Router (LiteLLM-style + local) | Any provider |
| Persistence | SQLite + files | Local-first |
| Skills | Web registry (HTTP) | On-demand, zero install |
| Voice | LiveKit / Pipecat style pipeline | Real-time |

---

## 8. Cross-Platform Reality

| Surface | Capability |
|---------|------------|
| Desktop / Mac (Tauri) | Full agents, local models, Docker sandboxes, heavy work |
| Android / iPhone (PWA) | Chat, Voice, Approve/Reject, Status, light resume |
| Optional always-on server | Background long-running tasks |

Mobile is deliberately a control plane, not a full execution environment.

---

## 9. Security Architecture

- Least privilege by default
- Permission Gateway is mandatory for all mutating tools
- Action classification: Read / Low-Risk / High-Risk / Destructive
- Secrets in encrypted store / OS keychain
- Full append-only audit log
- Sandbox isolation for code execution
- Prompt-injection resistance via structured tool calls and clear delimiting of untrusted content

---

## 10. Performance Principles

- Async I/O everywhere
- Context is treated as a scarce resource → summaries preferred over raw dumps
- Streaming responses to the UI
- Parallelism only after Shared Context is locked
- Local models preferred for tight loops when quality is sufficient
- Resource limits on sandboxes and agent count

---

## 11. Extensibility Points

- New Domain Sections via configuration + skills
- New specialist roles via skills + system prompts
- New tools via Tool Layer plugins
- New models via Model Router
- Custom Skills Registry URL

---

## 12. Relationship to Other Documents

| Document | Role |
|----------|------|
| `CLAUDE.md` | Single entry point for any coding agent |
| `FINAL-PRD.md` | Product requirements (what & why) |
| `FINAL-SYSTEM-DESIGN.md` | Earlier high-level design |
| `FINAL-CONSISTENCY-PROTOCOL.md` | Detailed Shared Context rules |
| `FINAL-LARGE-CODE-REVIEW-PROTOCOL.md` | Detailed review & fix protocol |
| `FINAL-AGENTS-AND-TEAMS.md` | Domain armies and agent roster |
| **This file (ARCHITECTURE.md)** | Structural and component reference |

---

**This architecture is designed so that parallel agents stay consistent, large codebases can be reviewed safely, and the system remains fast and controllable across devices.**
