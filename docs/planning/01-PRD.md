# Product Requirements Document (PRD)
## AetherForge — Autonomous Hierarchical Coding Agent

**Version:** 0.1  
**Status:** Draft for Review  
**Owner:** User + Planning Agent  

---

## 1. Problem Statement

Individual developers and small teams spend significant time on repetitive engineering work, context switching, and coordination. Existing AI coding tools are either:
- Single-agent and limited in scope, or
- Cloud-only and expensive / privacy-sensitive, or
- Lack strong hierarchical orchestration and Spec-Driven discipline.

Users need a personal, always-available, hierarchical AI engineering organization that can plan, code, test, and manage GitHub repositories under explicit human control, running on their own devices and preferred models (including fully local).

---

## 2. Goals

### Primary Goals
1. Enable a single user to operate as if they have a 10–50 person engineering team (Main Lead + specialists).
2. Enforce Spec-Driven Development so that code is generated against clear, reviewed specifications.
3. Support any LLM (cloud API or local open-source) without vendor lock-in.
4. Provide a ChatGPT-class conversation experience with full history and resume.
5. Require explicit permission before non-trivial actions.
6. Offer real-time Voice Mode for hands-free direction.
7. Work across Desktop, Laptop, Android, and iPhone (with realistic mobile scope).

### Non-Goals (v1)
- Fully autonomous overnight unattended operation without any approval policy.
- Replacing the user’s judgment on architecture or product decisions.
- Running heavy long-running agents primarily on mobile devices.
- Multi-user / team collaboration features (single-user first).

---

## 3. Target Users

- Individual software engineers, indie hackers, technical founders.
- Data scientists / ML engineers who also write production code.
- Developers who want local-first or hybrid AI workflows.
- Users comfortable granting GitHub and limited device permissions.

---

## 4. User Personas (Simplified)

**Primary:** “Solo Builder” — builds full-stack + ML products alone, wants to multiply output without losing control or privacy.

**Secondary:** “Privacy-Conscious Engineer” — prefers local models and hates sending entire codebases to third-party clouds.

---

## 5. Core User Journeys

1. **Onboarding**
   - Install / open app (Desktop first).
   - Connect GitHub (PAT or OAuth with minimal scopes).
   - Configure preferred models (API keys + local endpoints).
   - Grant device permissions (filesystem scopes, terminal if needed).
   - Optional: import custom skills / instructions.

2. **Start Work**
   - Open new chat or resume previous.
   - Describe high-level goal in natural language (or voice).
   - Main Agent proposes Spec → user reviews/edits → Design → Tasks.
   - User approves plan (or adjusts).
   - Main Agent assigns work to Sub-Agents.
   - User receives progress updates and approval requests for significant actions.

3. **Voice Session**
   - User starts Voice Mode.
   - Conversational direction while agents work in background.
   - Real-time status and confirmation requests spoken back.

4. **Cross-Device**
   - Start task on laptop.
   - Check status / approve from phone.
   - Resume conversation on another device.

5. **Settings & Customization**
   - Change models, add skills, adjust permission policies, manage connected devices/repos.

---

## 6. Functional Requirements

### 6.1 Conversation System
- Sidebar with conversation list (searchable, resumable).
- New Chat, archive, delete.
- Streaming responses.
- Ability to attach files, images, or existing specs.
- Markdown + code rendering.
- Explicit “Resume” of previous multi-agent sessions.

### 6.2 Spec-Driven Workflow
- Main Agent can generate / update:
  - Product Spec / Requirements
  - Technical Design
  - Task Breakdown
- User can edit any of these artifacts before implementation begins.
- Implementation only proceeds against approved tasks.
- Verification step against acceptance criteria.

### 6.3 Agent Hierarchy
- One Main Agent (Software Engineering Lead).
- Configurable set of Sub-Agents with roles (see Agents Instructions).
- Main Agent can spawn, assign, pause, or terminate Sub-Agents.
- Sub-Agents report status and results back to Main Agent.
- Parallel execution where safe.

### 6.4 GitHub Integration
- Authenticate via Personal Access Token or OAuth (user-controlled scopes).
- Create / delete / archive repositories (with strong confirmation).
- Clone, branch, commit, push, open PR, comment, manage issues.
- Read repository contents, issues, PRs, Actions status.
- All write operations require appropriate permission level.

### 6.5 Device & Execution
- Desktop: full local sandbox (Docker or equivalent) + optional native terminal.
- Mobile: control plane only (view, chat, voice, approve). Optional lightweight local actions later.
- Remote sandbox option (user-provided or integrated).
- File operations limited to user-approved directories.

### 6.6 Model Management
- Support any OpenAI-compatible API endpoint.
- Native support for major providers + local (Ollama, etc.).
- Per-agent or global model selection.
- Fallback chains (primary → secondary → local).
- Cost and token tracking.

### 6.7 Permissions & Safety
- Action classification: Read / Low-Risk Write / High-Risk / Destructive.
- Configurable policies (auto-approve Read, ask for Write, always ask for Destructive).
- Explicit confirmation UI/voice for high-risk actions.
- Full audit log of every tool call and decision.
- Ability to revoke GitHub token and device permissions instantly.

### 6.8 Voice Mode
- Real-time speech-to-text and text-to-speech.
- Interruption support.
- Spoken status updates and confirmation requests.
- Works while agents are executing tasks.

### 6.9 Skills & Custom Instructions
- Global and per-agent Markdown skill files.
- User can add, edit, enable/disable skills from Settings.
- Skills can include tool definitions or procedural knowledge.

### 6.10 Settings
- GitHub connection.
- Model / API configuration.
- Device permissions and trusted directories.
- Permission policies.
- Voice settings.
- Agent roster and role configuration.
- Theme / language (English + Roman Urdu support recommended).

---

## 7. Non-Functional Requirements

- **Privacy:** Code and conversations stay local by default. No telemetry of code content without explicit opt-in.
- **Performance:** Local models preferred for latency-sensitive loops. Streaming UI always.
- **Reliability:** Graceful degradation when models or network fail. Session resume after crash.
- **Observability:** Every significant decision and tool call is logged and inspectable.
- **Extensibility:** New Sub-Agent types and tools can be added via configuration + skills.
- **Security:** See dedicated Security document. Principle of least privilege.

---

## 8. Success Metrics (Initial)

- User can complete a non-trivial feature (Spec → working PR) with < 30% manual coding.
- Permission prompts are clear and not overly frequent for trusted workflows.
- Voice Mode usable for at least 10-minute continuous sessions.
- System runs usefully with fully local models on a modern laptop.
- Zero unauthorized GitHub or filesystem actions.

---

## 9. Open Questions for User

1. Preferred primary runtime for code execution (local Docker vs remote sandbox vs both)?
2. Minimum viable Sub-Agent set for v1?
3. Hard requirement for fully offline GitHub simulation, or network is acceptable?
4. Preferred UI stack direction (web-based cross-platform vs native)?
5. Any specific languages / stacks that must be first-class (Python, TypeScript, etc.)?

---

**End of PRD v0.1**
