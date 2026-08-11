# Shared Context & Consistency Protocol
**Version:** 1.0  
**Priority:** Critical (solves the #1 failure mode)

---

## Problem Statement

When multiple agents work in parallel on the same project (especially creative work like games, UIs, or multi-module systems), they independently invent theme, colors, style, naming, and architectural details. Result: visual and structural inconsistency (example: night-time sky created by one agent while the sniper character created by another cannot see anything).

---

## Solution: Mandatory Shared Project Context

No specialist agent is allowed to begin implementation work until a **Shared Project Context** document exists and has been approved (or auto-approved under policy).

### Contents of Shared Project Context

```markdown
# Shared Project Context — [Project Name]
Version: X
Approved: yes/no

## 1. Theme & Mood
- Overall theme (e.g. cyberpunk night, clean minimal SaaS, cartoon adventure...)
- Mood keywords

## 2. Visual Style Guide
- Color palette (exact hex values + usage rules)
  - Primary: #...
  - Secondary: #...
  - Background: #...
  - Text: #...
  - Accent / Danger / Success: #...
- Typography rules
- Spacing / density rules
- Icon / illustration style (if any)

## 3. Architecture Decisions
- High-level structure
- Key technology choices
- Module boundaries
- Naming conventions

## 4. Constraints & Non-Negotiables
- Things agents must never change or invent
- Performance budgets
- Accessibility / platform requirements

## 5. Glossary
- Important terms and their exact meaning in this project

## 6. Acceptance Criteria Snapshot
- What “done” looks like for the current milestone
```

---

## Process

1. User states goal.
2. Main Orchestrator (or Planning Section) produces Spec + Design + **Shared Project Context**.
3. User reviews / edits / approves the Shared Project Context.
4. Only after approval does the Orchestrator activate Domain Sections and spawn specialists.
5. Every specialist task assignment includes the current Shared Project Context as mandatory reading.
6. If during work a specialist discovers a need to change theme/architecture, it must escalate to Main Orchestrator instead of inventing.

---

## Enforcement

- System prompt of every Sub-Agent contains:  
  “You must follow the Shared Project Context exactly. Do not invent colors, themes, or architectural patterns. If something is missing, report to the Orchestrator.”
- Skills for creative domains (Game, Frontend, Content) explicitly reference and reinforce the Shared Context.
- Review agents check compliance with Shared Context as part of their checklist.

---

## Benefits

- Parallel work becomes safe.
- Results look like they came from one coherent team.
- User only has to approve the high-level vision once.
- Drastically reduces rework.
