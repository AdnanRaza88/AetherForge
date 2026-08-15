# AetherForge — Project Context for Coding Agents (CLAUDE.md)

**Read this file first. It is the single source of truth for understanding the entire project without exploring every document.**

---

## What is AetherForge?

AetherForge is an autonomous hierarchical multi-agent coding system.  
One Main Orchestrator leads multiple Domain Armies (Game, Backend, Frontend, ML, DL, Data Science, Content, Security, etc.).  
It is Spec-Driven, model-agnostic (any cloud API + local models), permission-first, and designed for Desktop + Mac + Android + iPhone (mobile is control plane).

Primary language of the agent runtime: **Python**.  
UI: Tauri (desktop) + PWA (mobile), liquid-glass / glassmorphism style.  
Code execution: local Docker sandbox (primary) + remote option.

---

## Non-Negotiable Rules

1. **Spec-Driven Development**  
   Non-trivial work always starts with Spec → Design → Shared Project Context → Tasks. No specialist coding before these exist and are approved.

2. **Shared Project Context is Mandatory**  
   Before any parallel work, a Shared Project Context document (theme, exact colors, style guide, architecture decisions, constraints) must exist. Every agent receives it. Agents must never invent their own theme or colors.

3. **Large Code Review Protocol**  
   Structure first → Partition → Parallel specialized reviewers (summaries only) → Aggregator → Small verified fixes. Never dump entire large codebases into one agent context.

4. **Skills are Dynamic**  
   Skills are fetched on-demand from a web Skills Registry. No heavy local installs. Agents load only what they need.

5. **Permission-First**  
   Every non-trivial side effect goes through the Permission Gateway. Destructive actions always require explicit approval.

6. **Clean Human Code Style**  
   No AI filler comments, no emojis in code or commits, small focused functions, consistent naming, senior-engineer quality.

---

## Key Documents (in order of importance)

| Document | Purpose |
|----------|---------|
| `docs/ARCHITECTURE.md` | **Main architecture reference** (components, flows, stack) |
| `docs/FEATURES-AND-SERVICES.md` | Complete inventory of features & internal services |
| `docs/FOLDER-STRUCTURE.md` | Official monorepo folder + file layout (MCPs, plugins, skills…) |
| `docs/planning/final/FINAL-PRD.md` | Locked product requirements |
| `docs/planning/final/FINAL-SYSTEM-DESIGN.md` | Earlier high-level design |
| `docs/planning/final/FINAL-CONSISTENCY-PROTOCOL.md` | How we prevent theme/style drift |
| `docs/planning/final/FINAL-LARGE-CODE-REVIEW-PROTOCOL.md` | How we review & fix large codebases safely |
| `docs/planning/final/FINAL-AGENTS-AND-TEAMS.md` | Domain armies and agent roles |
| `docs/planning/08-security-privacy.md` | Security model |
| `docs/planning/09-ux-frontend-spec.md` | UI / glassmorphism direction |

---

## How Agents Should Behave

- Main Orchestrator: plans, creates Shared Context, routes to the correct Domain Section, monitors, aggregates.
- Section Leads: manage specialists inside their domain.
- Specialists: stay in scope, follow Shared Context strictly, produce structured results + short summaries.
- Reviewers: produce findings + summaries only. Never return full source unless explicitly asked for a tiny snippet.
- When in doubt about theme, colors, or architecture → escalate, do not invent.

---

## Current Status

Planning phase is complete (v1.0 final documents).  
Implementation has not started.  
Next: Phase 0 — core runtime + Main Orchestrator + one Domain Section proof-of-concept.

---

## Quick Start for Any Coding Agent

1. Read this CLAUDE.md completely.
2. Read `docs/ARCHITECTURE.md` (main structural reference).
3. Read FINAL-PRD.md.
4. Read the two Protocol documents (Consistency + Large Code Review).
5. Only then look at agent instructions or source code.
6. All new work must follow Spec-Driven process and Shared Context rules.

**This file exists so you do not need to explore the entire repository to understand the project.**
