# System Design
## AetherForge

**Version:** 0.1  
**Status:** Draft

---

## 1. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        User Surfaces                                 │
│  Desktop App (primary)   │   Mobile App (control)   │   Voice       │
│  Web UI (optional)       │   (chat / approve /      │   Channel     │
└──────────────┬───────────┴──────────┬───────────────┴───────┬───────┘
               │                      │                       │
               └──────────────────────┼───────────────────────┘
                                      │
                          ┌───────────▼───────────┐
                          │   Control Plane       │
                          │  (Conversation +      │
                          │   Permission +        │
                          │   Session Manager)    │
                          └───────────┬───────────┘
                                      │
               ┌──────────────────────┼──────────────────────┐
               │                      │                      │
    ┌──────────▼──────────┐  ┌────────▼────────┐  ┌──────────▼──────────┐
    │   Main Agent        │  │  Memory &       │  │  Model Router       │
    │   (Orchestrator)    │  │  State Store    │  │  (API + Local)      │
    └──────────┬──────────┘  └─────────────────┘  └─────────────────────┘
               │
               │ assigns / monitors
               ▼
    ┌──────────────────────────────────────────────────────────┐
    │              Sub-Agent Pool (dynamic)                     │
    │  Backend │ Frontend │ ML/DS │ Test │ DevOps │ Security │  │
    │  Docs │ Research │ Reviewer │ ...                         │
    └──────────────────────┬───────────────────────────────────┘
                           │
               ┌───────────┼───────────┐
               │           │           │
    ┌──────────▼──┐  ┌─────▼─────┐  ┌──▼────────────┐
    │  Tool Layer │  │  Sandbox  │  │  GitHub Client│
    │  (file,     │  │  Runtime  │  │  (Octokit +   │
    │   shell,    │  │  (Docker  │  │   custom)     │
    │   browser,  │  │   / remote)│  └───────────────┘
    │   search)   │  └───────────┘
    └─────────────┘
```

---

## 2. Core Components

### 2.1 Control Plane
- Conversation Manager (threads, history, resume)
- Permission Gateway (classifies actions, enforces policy, requests approval)
- Session & Project Context Manager
- Voice Gateway (STT / TTS, barge-in)
- Settings & Secrets Store (encrypted)

### 2.2 Main Agent (Orchestrator)
- Receives user goal.
- Drives Spec → Design → Tasks workflow.
- Decomposes work and assigns to Sub-Agents.
- Monitors progress, handles failures, requests re-planning.
- Aggregates results and reports to user.
- Rarely writes large code itself; focuses on planning, review, and coordination.

### 2.3 Sub-Agent Pool
- Role-specialized agents with narrow system prompts and tool subsets.
- Can be long-lived or ephemeral (spawned per task).
- Report structured status and artifacts back to Main Agent.
- Run inside isolated execution contexts when performing side effects.

### 2.4 Model Router
- Unified interface for all LLM calls.
- Supports cloud APIs and local endpoints.
- Fallback chains, cost tracking, token budgeting.

### 2.5 Tool Layer
- File system (scoped), Shell (sandboxed), Git, GitHub API, Web search, Code execution, Skills.

### 2.6 Sandbox Runtime
- Primary: Docker-based isolated environments (OpenHands style).
- Remote sandbox option.

### 2.7 Memory & State
- Conversation history, Project context, Agent working memory, optional vector memory.
- Local-first with optional encrypted sync.

### 2.8 GitHub Integration Layer
- PAT or OAuth, full repo lifecycle, all mutating calls through Permission Gateway.

---

## 3. Cross-Device Strategy

| Device Type | Role | Capabilities |
|-------------|------|--------------|
| Desktop / Laptop | Primary Execution + Control | Full agents, sandboxes, local models |
| Android / iPhone | Control Plane | Chat, Voice, status, approve/reject |
| Optional Server | Always-on Execution | Long-running agents |

---

## 4. Technology Direction (Proposed)

- Frontend: Tauri/Electron + React/Svelte or web + PWA
- Agent Runtime: Python preferred
- Sandbox: Docker
- LLM: LiteLLM + local clients
- Voice: LiveKit / Pipecat style
- Storage: SQLite local

---

**End of System Design v0.1**
