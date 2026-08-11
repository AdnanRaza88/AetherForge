# AetherForge — Final System Design
**Version:** 1.0  

---

## 1. High-Level Architecture

```
User (Desktop / Mac / Android / iPhone)
          │
          ▼
┌─────────────────────────────────────┐
│         Control Plane               │
│  Conversation • Permission Gateway  │
│  Session • Voice • Settings         │
└─────────────────┬───────────────────┘
                  │
      ┌───────────┴───────────┐
      │                       │
┌─────▼──────┐         ┌──────▼──────┐
│ Main       │         │ Model       │
│ Orchestrator│◄───────►│ Router      │
│ (Lead)     │         │ (any LLM)   │
└─────┬──────┘         └─────────────┘
      │
      │ routes to Section
      ▼
┌─────────────────────────────────────┐
│         Domain Sections / Armies     │
│  Game │ Backend │ Frontend │ ML │    │
│  DL │ DS │ Content │ Security │ ...  │
│  Each has Section Lead + Specialists │
└─────────────────┬───────────────────┘
                  │
      ┌───────────┼───────────┐
      ▼           ▼           ▼
 Tool Layer   Sandbox      Skills
 (file,shell,  Runtime     Registry
  git,gh)     (Docker /    (web fetch)
              Remote)
```

---

## 2. Core Runtime Decisions

- **Language:** Python (async, high performance with proper design)
- **UI:** Single web codebase → Tauri (Desktop/Mac) + PWA (Android/iPhone)
- **Sandbox:** Local Docker primary + optional remote sandboxes
- **Skills:** On-demand HTTP fetch from configurable Skills Registry URL
- **Persistence:** Local SQLite + file-based artifacts (Specs, Shared Context, summaries)

---

## 3. Shared Context Protocol (Critical)

Before any parallel implementation:

1. Main Orchestrator (or Planning Section) produces a **Shared Project Context** document containing:
   - Theme & Visual Style Guide
   - Exact Color Palette (hex values)
   - Typography & UI rules
   - Architecture decisions
   - Constraints & non-negotiables
   - Glossary of terms
   - Acceptance criteria summary

2. This document is versioned and approved by the user (or auto-approved under policy).

3. Every Sub-Agent in every Section receives this document as mandatory context for the task.

4. No specialist is allowed to invent its own theme, colors, or architectural style.

This directly solves the “dark sky + blind sniper” class of consistency failures.

---

## 4. Domain Section Model

User query → Main Orchestrator classifies intent → activates one or more Sections.

Each Section:
- Has a Section Lead (mid-level orchestrator)
- Owns a roster of specialist agents
- Owns section-specific skills
- Can request cross-section help (e.g. Game Section asks Frontend + Backend)

Sections are extensible via configuration + skills. New armies can be added without core code changes.

---

## 5. Large Code Review & Safe Fix Protocol

Triggered when the task involves reviewing or fixing a large existing codebase.

1. **Structure Explorer Agent**  
   Maps directory tree, key modules, ownership, entry points. Produces structure summary only.

2. **Partitioner**  
   Splits the codebase into coherent partitions (by module, feature, or layer).

3. **Parallel Reviewer Agents** (N agents, N decided by size)  
   Each reviewer receives only its partition + Shared Context + review checklist.  
   Outputs: structured findings + short summary. Never returns full code.

4. **Summary Aggregator Agent**  
   Merges all reviewer summaries into a clean, prioritized report (bugs by severity, suggested fix order).

5. **Fix Phase** (optional)  
   Small, isolated fixes applied one by one.  
   After each fix: targeted re-review of the changed partition.  
   Never attempt a giant “fix everything” pass in one context.

This keeps every agent’s context window focused and prevents cascade of new bugs.

---

## 6. Skills System

- Skills live in a web-accessible registry (user-configurable URL or default).
- Format: Markdown with optional front-matter (tools, models, category).
- Agent requests skill by name/category → lightweight fetch → inject into context.
- No local package installation required for skills.
- Core system skills (planning templates, consistency rules, review checklists) are always available.

---

## 7. Permission & Safety

- Action classes: Read / Low-Risk Write / High-Risk / Destructive
- Permission Gateway sits in front of every tool that has side effects
- User can set policies (auto-allow read, always ask for destructive, etc.)
- Full audit log

---

## 8. Performance Principles

- Async everywhere for I/O
- Context is managed aggressively (summaries > raw dumps)
- Streaming responses
- Local models preferred for tight loops when quality allows
- Parallel agents only when independence is guaranteed by Shared Context

---

## 9. Cross-Device Reality

| Surface | Role |
|---------|------|
| Desktop / Mac (Tauri) | Full power: agents, sandboxes, local models |
| Android / iPhone (PWA) | Control plane: chat, voice, approve, status, light resume |
| Optional always-on server | Long-running background work |

---

**This design is optimized for consistency, safe large-scale work, and real-world multi-platform use.**
