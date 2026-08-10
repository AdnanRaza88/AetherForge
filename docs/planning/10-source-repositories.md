# Source & Inspiration Repositories
## AetherForge

Curated list of high-signal open-source projects that inform architecture, agent patterns, sandboxing, multi-agent orchestration, and UI direction.

---

## Primary Coding Agent Platforms

| Repository | Stars (approx) | Why relevant |
|------------|----------------|--------------|
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | ~83k | Leading open-source autonomous SE agent. Docker sandbox, event-stream architecture, multi-agent support, PR workflow. Closest production reference for sandbox + agent loop. |
| [Aider-AI/aider](https://github.com/Aider-AI/aider) | ~48k | Git-native pair programming. Repo map via Tree-sitter, automatic commits, model-agnostic. Excellent reference for clean git integration and human-in-the-loop editing. |
| [aaif-goose/goose](https://github.com/block/goose) (Block / Linux Foundation) | ~50k+ | Extensible agent with deep MCP support, local models, recipes. Strong model-agnostic and extension design. |
| [cline/cline](https://github.com/cline/cline) | ~65k | Approval-first coding agent (VS Code + CLI). Good permission / human approval patterns. |
| [continuedev/continue](https://github.com/continuedev/continue) | ~35k | Open coding agent with strong local model support and IDE integration. |

---

## Multi-Agent Orchestration Frameworks

| Repository | Stars (approx) | Why relevant |
|------------|----------------|--------------|
| [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) | ~55k | Role-based hierarchical crews, task delegation, memory. Direct inspiration for Main Agent + specialist Sub-Agents. |
| [FoundationAgents/MetaGPT](https://github.com/FoundationAgents/MetaGPT) | ~69k | Software company simulation (PM, Architect, Engineer roles). SOP-driven multi-agent software development. |
| [microsoft/autogen](https://github.com/microsoft/autogen) | ~60k | Conversational multi-agent framework. Useful patterns for agent-to-agent messaging. |
| [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) | ~33k+ | Stateful graph-based orchestration. Good for complex control flows and recovery. |

---

## Spec-Driven & Agent Harness References

- GitHub Spec Kit (Microsoft) — Spec → Plan → Tasks workflow that we adopt as core process.
- SWE-agent (Princeton) — Agent-Computer Interface patterns.
- OpenCode, Hermes Agent, OpenClaw — additional harness and skill-system ideas.

---

## UI / Glassmorphism Direction

We will follow liquid-glass / glassmorphism principles (frosted translucent panels, soft blur, restrained depth, light theme, minimal chrome, no card spam, typography-first). Implementation will use modern CSS (backdrop-filter, careful shadows) and stay clean and human-feeling.

Code style target: senior-human written — no AI filler, no unnecessary comments, consistent naming, small focused functions, no emoji in code or commits.

---

## How We Will Use These Sources

- Study architecture and patterns, do not copy large portions of code.
- Prefer MIT / Apache-2.0 licensed projects for any direct reuse.
- All new code for AetherForge will be written cleanly and attributed where patterns are adapted.
- Sandbox design will draw heavily from OpenHands-style isolation.
- Orchestration and role model will draw from CrewAI + MetaGPT ideas, implemented with our own thinner control plane.
