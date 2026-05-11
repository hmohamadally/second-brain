---
type: project
name: AI Engineering Command Center
status: active
start_date: 2026-05-10
tags: [hermes, copilot, obsidian, gbrain, infrastructure, meta]
---

# Project — AI Engineering Command Center

## Overview

Plateforme personnelle d'ingénierie IA avec orchestration agentique, mémoire persistante, capitalisation de connaissances et amélioration cumulative. Le système permet de développer des solutions logicielles de manière fortement augmentée par des agents IA.

## Stack

- Language: Python 3.11
- Agent: Hermes Agent v0.8.0
- LLM (orchestration): GPT-5.4 via GitHub Copilot API
- LLM (génération): Claude Opus 4.6 (VS Code Copilot Chat)
- Knowledge: Obsidian vault + GBrain (PGLite, nomic-embed-text 768d)
- MCP: filesystem, gbrain, github (3 serveurs, ~113 tools)
- IDE: VS Code + ACP Client

## Architecture

- [[adr/ADR-001-architecture-ai-command-center]]

## Infrastructure

| Composant | Path | Status |
|-----------|------|--------|
| Hermes Agent | `~/Hermes/` | ✅ Installé |
| Obsidian Vault | `~/second-brain/` | ✅ Créé |
| GBrain | `~/.gbrain/brain.pglite` | ✅ 22 pages, 768d embeddings |
| MCP filesystem | vault access | ✅ 14 tools |
| MCP gbrain | knowledge retrieval | ✅ 30 tools |
| MCP github | repo operations | ✅ 26 tools |
| ACP VS Code | editor integration | ✅ Configuré |

## Phases

| Phase | Objectif | Status |
|-------|----------|--------|
| 1 — Fondation | Base opérationnelle | ✅ Done |
| 2 — Standardisation | Workflows + mémoire structurée | ✅ Done |
| 3 — Automatisation | Boucles d'ingénierie auto | ✅ Done |
| 4 — Retrieval avancé | Semantic search + embeddings | ✅ Done |

## Repos

- GitHub: [hmohamadally/Hermes](https://github.com/hmohamadally/Hermes)
- Upstream: [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

## Key Patterns Used

- [[patterns/mcp-multi-server-architecture]] — Architecture MCP découplée

## Retrospectives

- [[retrospectives/2026-05-10-setup-initial]] — Setup initial

---

## Timeline

- 2026-05-10: Phase 1 complétée (vault, MCPs, GBrain, ACP)
- 2026-05-10: Phase 2 complétée (ADR-001, workflows, conventions)
- 2026-05-11: Phase 3 complétée (Copilot API GPT-5.4, embeddings Ollama)
- 2026-05-11: Phase 4 complétée (hybrid search, 768d vectors, E2E test OK)
- 2026-05-11: Vault enrichi (20 liens, 13 timeline entries, Related sections)


## Related

- [[adr/ADR-001-architecture-ai-command-center]] — Architecture fondatrice
- [[tasks/TASK-001-setup-ai-command-center]] — Task d'implémentation initiale
- [[retrospectives/2026-05-10-setup-initial]] — Retro du setup
- [[patterns/mcp-multi-server-architecture]] — Pattern MCP extrait
- [[workflows/ai-engineering-setup]] — Workflow de setup
- [[workflows/feature-development]] — Workflow de dev standard
- [[snippets/vault-auto-sync]] — Script de sync automatique
