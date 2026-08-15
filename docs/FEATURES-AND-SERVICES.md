# AetherForge — Complete Features & Services Inventory
**Version:** 1.0  
**Date:** 2026-08-15  
**Status:** Final for Architecture Phase

This document lists every major feature and internal service the system must support.  
It is the foundation for designing the folder/file structure, MCPs, plugins, and settings.

---

## 1. Core User-Facing Features

### 1.1 Conversation System
- New chat / resume chat / archive / delete
- Sidebar conversation history (searchable)
- Streaming responses
- Markdown + code block rendering
- File / image attachment
- Multi-agent session visibility (which agents are working)
- Explicit “Resume multi-agent session”

### 1.2 Spec-Driven Workflow
- Generate Spec (Requirements + Acceptance Criteria)
- Generate Design
- Generate Shared Project Context (theme, colors, architecture, constraints)
- Generate Task breakdown
- User edit + approve gates at each stage
- Living documents (versioned)

### 1.3 Domain Armies / Multi-Agent System
- Main Orchestrator
- Domain Section Leads (Game, Backend, Frontend, ML, DL, DS, Content, Security, DevOps, Research…)
- Specialist Sub-Agents
- Dynamic spawning of agents
- Parallel execution with Shared Context injection
- Progress monitoring and failure recovery
- Structured result + summary reporting

### 1.4 Large Code Review & Safe Fix
- Structure Explorer
- Automatic partitioning
- Parallel specialized reviewers (summaries only)
- Summary Aggregator
- Safe Fixer (small verified changes + re-review)

### 1.5 GitHub Integration
- Authenticate (PAT / OAuth)
- Create / delete / archive repositories
- Branch, commit, push
- Create / update Pull Requests
- Issues management
- Read repository contents, Actions status
- All mutating operations behind Permission Gateway

### 1.6 Voice Mode
- Real-time Speech-to-Text
- Text-to-Speech
- Barge-in / interruption support
- Spoken status updates and confirmation requests
- Works while agents run in background

### 1.7 Permissions & Safety
- Action classification (Read / Low-Risk Write / High-Risk / Destructive)
- Configurable policies
- Explicit approval UI + voice confirmation
- Full audit log
- Instant revoke of tokens and permissions

### 1.8 Settings & Configuration
- GitHub connection
- Model providers & API keys / local endpoints
- Per-role or global model preferences
- Permission policies
- Trusted directories / filesystem scopes
- Skills Registry URL
- Voice settings
- Agent roster / enable-disable sections
- Theme / language
- Cost budgets

### 1.9 Cross-Platform
- Desktop / Mac (Tauri) — full power
- Android / iPhone (PWA) — control plane (chat, voice, approve, status)
- Session resume across devices

### 1.10 Skills System
- Dynamic loading from web Skills Registry
- Category-specific skills (game, ml, review, planning templates…)
- On-demand fetch (no heavy install)
- User can add custom skills

---

## 2. Internal Services (Backend Capabilities)

| Service | Responsibility |
|---------|----------------|
| **Conversation Service** | Thread management, message history, streaming |
| **Orchestrator Service** | Main agent loop, routing, task graph |
| **Section Manager** | Domain army lifecycle, specialist spawning |
| **Model Router Service** | Unified LLM calls, fallbacks, cost tracking |
| **Permission Gateway** | Action classification, policy enforcement, approval flow |
| **Sandbox Manager** | Docker / remote sandbox lifecycle, resource limits |
| **Tool Registry** | Available tools, schema validation, execution |
| **Skills Loader** | Fetch + cache skills from registry |
| **GitHub Client** | All GitHub API operations |
| **File System Service** | Scoped read/write inside trusted dirs or sandbox |
| **Memory / State Service** | SQLite + file artifacts (Specs, Shared Context, summaries) |
| **Audit Logger** | Append-only event log |
| **Voice Gateway** | STT / TTS pipeline |
| **Settings Service** | Encrypted secrets, user preferences |
| **Cost Tracker** | Token usage and estimated cost per session/agent |
| **Notification Service** | Progress, approval requests, completion alerts |

---

## 3. MCP (Model Context Protocol) Requirements

MCPs will be used for:

- **Filesystem MCP** — controlled file access
- **Git / GitHub MCP** — repository operations
- **Shell / Terminal MCP** — sandboxed command execution
- **Browser MCP** (optional) — web research
- **Custom AetherForge MCP** — internal tools (permission checks, skill loading, sandbox control, Shared Context injection)

Plugins / MCP servers should be isolatable and configurable.

---

## 4. Plugin System

- Tool plugins (extend Tool Layer)
- Skill plugins (extra skills packages)
- Domain Section plugins (new armies)
- Model provider plugins
- UI theme / component plugins (future)

---

## 5. System Settings Categories

1. **Identity & Auth** — GitHub token, user profile
2. **Models** — Providers, keys, local endpoints, defaults, fallbacks
3. **Permissions** — Policies per action class, trusted paths
4. **Agents & Sections** — Enable/disable armies, custom instructions
5. **Skills** — Registry URL, enabled skills, custom skills
6. **Sandbox** — Docker settings, resource limits, remote sandbox config
7. **Voice** — STT/TTS engine, language, sensitivity
8. **Appearance** — Theme (liquid glass), language (EN + Roman Urdu)
9. **Advanced** — Cost budgets, logging level, offline mode preferences
10. **Data** — Export, clear history, backup

---

## 6. Non-Functional Features

- Local-first data
- Streaming UI
- Session persistence & resume
- Offline degradation (local models + queued GitHub actions)
- Full auditability
- Clean human-written code style enforcement (via skills + review agents)
- Fast startup and responsive agent loops

---

**This inventory is complete for v1 architecture design.**  
Any new feature must be added here before implementation.
