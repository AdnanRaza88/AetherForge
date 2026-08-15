# AetherForge — Folder & File Architecture
**Version:** 1.0  
**Date:** 2026-08-15  
**Status:** Final Recommendation

This is the recommended monorepo structure for the entire AetherForge system.  
It is designed for:
- Clear separation of concerns
- Easy location of MCPs, plugins, skills, settings
- Scalable Domain Armies
- Clean Python backend + modern frontend
- Spec-Driven development artifacts living alongside code

---

## Top-Level Structure

```
AetherForge/
├── CLAUDE.md                          # Single entry point for any coding agent
├── README.md
├── LICENSE
├── pyproject.toml                     # Python project config
├── package.json                       # Frontend / Tauri workspace root (if needed)
├── .env.example
├── .gitignore
│
├── docs/                              # All human + agent documentation
│   ├── ARCHITECTURE.md
│   ├── FEATURES-AND-SERVICES.md
│   ├── FOLDER-STRUCTURE.md            # This file
│   ├── planning/                      # Historical + final planning docs
│   │   └── final/
│   └── ...
│
├── apps/                              # Runnable applications
│   ├── desktop/                       # Tauri desktop app
│   ├── web/                           # Shared web frontend (used by Tauri + PWA)
│   └── pwa/                           # PWA-specific assets / config (optional)
│
├── backend/                           # Core Python agent runtime
│   ├── aetherforge/                   # Main Python package
│   │   ├── __init__.py
│   │   ├── main.py                    # Entry point
│   │   ├── control_plane/             # Conversation, Permission, Session, Voice, Settings
│   │   ├── orchestrator/              # Main Orchestrator + task graph
│   │   ├── sections/                  # Domain Armies
│   │   │   ├── base.py
│   │   │   ├── game/
│   │   │   ├── backend/
│   │   │   ├── frontend/
│   │   │   ├── ml/
│   │   │   ├── dl/
│   │   │   ├── datascience/
│   │   │   ├── content/
│   │   │   ├── security/
│   │   │   ├── devops/
│   │   │   └── research/
│   │   ├── agents/                    # Core agent types (Reviewer, Explorer, Aggregator, Fixer…)
│   │   ├── models/                    # Model Router
│   │   ├── tools/                     # Tool Layer implementations
│   │   ├── sandbox/                   # Docker + remote sandbox management
│   │   ├── skills/                    # Skills Loader + local cache
│   │   ├── memory/                    # State, SQLite, artifact storage
│   │   ├── github/                    # GitHub client
│   │   ├── permissions/               # Permission Gateway + policies
│   │   ├── audit/                     # Audit logger
│   │   ├── voice/                     # Voice Gateway
│   │   └── utils/
│   ├── tests/
│   └── scripts/
│
├── mcps/                              # Model Context Protocol servers
│   ├── filesystem/
│   ├── github/
│   ├── shell/
│   ├── browser/                       # optional
│   ├── aetherforge-core/              # Internal tools (permission, skill load, sandbox control)
│   └── README.md
│
├── plugins/                           # Extensibility
│   ├── tools/                         # Extra tool plugins
│   ├── sections/                      # Extra Domain Section plugins
│   ├── model_providers/
│   └── skills_packs/                  # Optional bundled skill packs
│
├── skills/                            # Default / core skills (can also be served via registry)
│   ├── planning/
│   ├── consistency/
│   ├── review/
│   ├── game/
│   ├── ml/
│   └── ...
│
├── config/                            # Default configuration templates
│   ├── settings.default.yaml
│   ├── permissions.default.yaml
│   ├── sections.default.yaml
│   └── models.default.yaml
│
├── data/                              # Runtime data (gitignored)
│   ├── db/                            # SQLite
│   ├── artifacts/                     # Specs, Shared Context, summaries
│   ├── skills_cache/
│   └── logs/
│
├── scripts/                           # Dev & ops scripts
│   ├── dev.sh
│   ├── build.sh
│   └── ...
│
└── tests/                             # Top-level integration / e2e tests (optional)
```

---

## Detailed Rationale

### `backend/aetherforge/`
Pure Python package. Clear modules map 1:1 to the architecture components:
- `control_plane/` → Conversation, Permission Gateway, Session, Voice, Settings
- `orchestrator/` → Main Lead agent
- `sections/` → One folder per Domain Army (easy to add new armies)
- `agents/` → Shared agent implementations (Structure Explorer, Reviewer, Aggregator, Safe Fixer…)
- `models/` → Model Router
- `tools/` → Concrete tool implementations
- `sandbox/` → Docker + remote
- `skills/` → Loader + cache
- `memory/` → Persistence
- `github/` → GitHub client
- `permissions/` → Policy engine
- `audit/` → Logging
- `voice/` → STT/TTS

### `mcps/`
All Model Context Protocol servers live here.  
Each MCP is a small independent package that can be started as a server.  
This keeps the core clean and allows users to enable/disable MCPs.

### `plugins/`
Anything that extends the system without touching core code:
- New tools
- New Domain Sections
- New model providers
- Bundled skill packs

### `skills/`
Core skills that ship with the project.  
In production these can also be served from the web Skills Registry.  
Local copy is useful for offline and for development.

### `apps/`
- `web/` → Shared React/Svelte/Vue frontend (liquid glass UI)
- `desktop/` → Tauri wrapper around the web frontend
- PWA assets can live inside `web/` or a thin `pwa/` folder

### `config/`
Default YAML/JSON templates that are copied or merged into user settings on first run.

### `data/`
All runtime state. Completely gitignored.

### `docs/`
All documentation. Planning history stays under `docs/planning/`.  
Architecture, features, and folder structure live at `docs/` root for easy discovery.

---

## Key File Naming Conventions

- Python: `snake_case.py`
- Frontend: standard modern conventions (kebab or camel depending on framework)
- Config: `*.default.yaml` or `*.example.yaml`
- Skills: `skill-name.md` (with optional YAML front-matter)
- MCP servers: each in its own folder with `server.py` or `main.py`

---

## How New Features Are Added

1. Feature is added to `docs/FEATURES-AND-SERVICES.md`
2. If it needs new code:
   - New Domain Army → `backend/aetherforge/sections/<name>/`
   - New agent type → `backend/aetherforge/agents/`
   - New tool → `backend/aetherforge/tools/` + optional MCP
   - New skill → `skills/<category>/`
   - New setting → `config/` + Settings Service
3. Documentation updated
4. Tests added

---

## MCP & Plugin Loading Strategy

- On startup the system discovers enabled MCPs from config.
- Plugins are loaded from `plugins/` (and optionally from user plugin directory).
- Skills are fetched on-demand from the configured Skills Registry URL (with local cache).
- Core skills in `skills/` act as fallback / offline source.

---

## Recommended First Implementation Order (Phase 0)

```
backend/aetherforge/
├── main.py
├── control_plane/
│   ├── conversation.py
│   ├── permissions.py
│   └── session.py
├── orchestrator/
│   └── main_orchestrator.py
├── models/
│   └── router.py
├── memory/
│   └── store.py
├── skills/
│   └── loader.py
└── sections/
    └── base.py
```

Plus minimal `apps/web` for the conversation UI and `mcps/` skeleton.

---

**This structure is clean, scalable, and maps directly to the architecture.**  
It keeps MCPs, plugins, skills, settings, and domain armies clearly separated.
