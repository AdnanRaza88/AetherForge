# Large Codebase Review & Safe Fix Protocol
**Version:** 1.0  
**Priority:** Critical

---

## Problem Statement

A single powerful model is excellent at spotting bugs in code it can see.  
When the same model tries to fix a large codebase, context window pressure causes it to forget earlier parts, lose track of interactions, and introduce many new bugs while “fixing” the original ones.

---

## Solution: Hierarchical, Summary-Based, Partitioned Review

### Phase 1 — Structure Exploration (single agent)
- Map directory tree
- Identify major modules, entry points, data flow
- Produce a short structure summary + ownership map
- Never load full file contents yet

### Phase 2 — Partitioning
- Split the codebase into coherent, loosely-coupled partitions (by feature, layer, or module)
- Size of each partition chosen so it fits comfortably in a specialist’s context together with Shared Context + checklist

### Phase 3 — Parallel Specialized Review
- Spawn N Reviewer agents (N scales with codebase size; can be 5–20+)
- Each Reviewer receives:
  - Only its partition
  - Shared Project Context
  - Review checklist (security, correctness, performance, style, consistency with Shared Context)
- Each Reviewer outputs **only**:
  - Structured findings (severity, location, description)
  - Short summary (max ~300–500 tokens)
- Reviewers never return full source code

### Phase 4 — Aggregation
- Summary Aggregator agent receives all short summaries
- Produces one clean, prioritized report:
  - Critical / High / Medium / Low
  - Suggested fix order
  - Cross-partition issues flagged

### Phase 5 — Safe Fixing (optional)
- Fixes are applied one small change at a time (or one partition at a time)
- After each fix:
  - Targeted re-review of the changed partition
  - Quick regression check if tests exist
- Never attempt a giant multi-file “fix everything” in a single agent context

---

## Context Management Rules

- Prefer structured summaries and MD reports over raw code dumps
- Keep full source in the sandbox / filesystem; agents only load what they need
- Main Orchestrator and Section Leads work primarily with summaries
- When a fix is needed, the fixer agent is given only the relevant files + the specific finding

---

## Skills Required

- `code-structure-explorer`
- `partition-planner`
- `specialized-reviewer` (with domain variants: backend, frontend, security, etc.)
- `summary-aggregator`
- `safe-fixer`

These skills live in the Skills Registry and are loaded on demand.

---

## Outcome

- High-quality bug detection at scale
- Dramatically fewer new bugs introduced during fixing
- Context windows stay focused and reliable
- Process is auditable and resumable
