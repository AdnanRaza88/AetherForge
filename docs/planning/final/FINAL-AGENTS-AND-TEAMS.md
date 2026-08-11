# AetherForge — Agents, Domain Armies & Skills
**Version:** 1.0  

---

## 1. Hierarchy

```
Main Orchestrator (Software Engineering Lead)
       │
       ├── Domain Section Leads (one per army)
       │         │
       │         └── Specialist Sub-Agents
       │
       ├── Planning & Spec Section (always available)
       ├── Review & Security Section
       └── Cross-cutting helpers (Research, Docs, etc.)
```

---

## 2. Domain Armies (Sections)

The Main Orchestrator classifies the user request and activates the relevant army/armies.

| Section / Army | Purpose | Example Specialists |
|----------------|---------|---------------------|
| Game Development | Full game projects (2D/3D, mechanics, art direction consistency) | Game Architect, Gameplay, Level Design, Art Direction, Audio, Performance |
| Backend | APIs, services, databases, auth, scaling | API Engineer, Database, Auth, Performance |
| Frontend | UI, UX, design systems, accessibility | UI Engineer, Design System, Accessibility, State Management |
| Machine Learning | Classical ML, feature engineering, evaluation | Feature Engineer, Model Trainer, Evaluator |
| Deep Learning | Neural nets, training loops, inference | Architecture Designer, Training, Inference Optimization |
| Data Science | Analysis, pipelines, visualization, experiments | Analyst, Pipeline, Visualization |
| Content & Media | Docs, videos, marketing assets, copy | Technical Writer, Video Script, Asset Planner |
| Security & Review | Threat modeling, code review, safe fixing | Security Reviewer, Code Reviewer, Aggregator |
| DevOps & Infra | CI/CD, Docker, deployment, observability | Infra Engineer, CI Specialist |
| Research & Exploration | Codebase mapping, library research | Structure Explorer, Researcher |

New armies can be added via configuration + skills.

---

## 3. Core Agent Types (Cross-Cutting)

- **Main Orchestrator** — planning, routing, Shared Context ownership, final aggregation
- **Section Lead** — manages specialists inside one domain
- **Structure Explorer** — maps large codebases (used in review protocol)
- **Partitioner** — splits work into context-friendly chunks
- **Specialized Reviewer** — reviews one partition, outputs summary only
- **Summary Aggregator** — merges reviewer outputs into clean report
- **Safe Fixer** — applies one small verified change at a time
- **Skill Loader** — fetches required skills from the registry

---

## 4. Skills System

- Skills Registry = configurable web URL
- Skills are lightweight Markdown files (optional YAML front-matter for tools/models)
- Agent requests skill by name or category → fetch → inject
- Examples of critical skills:
  - `shared-context-template`
  - `consistency-enforcer`
  - `code-structure-explorer`
  - `partition-planner`
  - `specialized-reviewer-backend`
  - `specialized-reviewer-frontend`
  - `summary-aggregator`
  - `safe-fixer`
  - Domain-specific planning templates (game, ml, etc.)

Agents never assume a skill is installed locally; they always fetch when needed.

---

## 5. Context Rules for Every Agent

Every specialist system prompt must include:

- You must obey the current Shared Project Context. Never invent theme, colors, or architecture.
- Prefer short structured summaries over large code dumps.
- If the task is too large for your context, request partitioning.
- Escalate ambiguity instead of guessing.

---

## 6. Parallelism Rules

- Parallel agents are only launched after Shared Project Context is approved.
- Parallel work must be independent or have clear interfaces defined in the Design.
- Creative domains (Game, Frontend, Content) are especially strict about Shared Context.

---

**This document defines how the “army” is organized and how knowledge is shared.**
