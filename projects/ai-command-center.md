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
- LLM (orchestration): gemma4:e4b via Ollama (fallback) — frontier model recommandé
- LLM (génération): GitHub Copilot (Claude Sonnet, GPT-5)
- Knowledge: Obsidian vault + GBrain (PGLite)
- MCP: filesystem, gbrain, github
- IDE: VS Code + ACP Client

## Architecture

- [[adr/ADR-001-architecture-ai-command-center]]

## Infrastructure

| Composant | Path | Status |
|-----------|------|--------|
| Hermes Agent | `~/Hermes/` | ✅ Installé |
| Obsidian Vault | `~/second-brain/` | ✅ Créé |
| GBrain | `~/.gbrain/brain.pglite` | ✅ 10 pages |
| MCP filesystem | vault access | ✅ 14 tools |
| MCP gbrain | knowledge retrieval | ✅ 30 tools |
| MCP github | repo operations | ✅ 26 tools |
| ACP VS Code | editor integration | ✅ Configuré |

## Phases

| Phase | Objectif | Status |
|-------|----------|--------|
| 1 — Fondation | Base opérationnelle | ✅ Done |
| 2 — Standardisation | Workflows + mémoire structurée | 🔄 En cours |
| 3 — Automatisation | Boucles d'ingénierie auto | ⏳ Planned |
| 4 — Retrieval avancé | Semantic search + embeddings | ⏳ Planned |

## Repos

- GitHub: [hmohamadally/Hermes](https://github.com/hmohamadally/Hermes)
- Upstream: [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)

## Key Patterns Used

- [[patterns/]] (à venir)

## Retrospectives

- (à venir après première feature)

---

## Timeline

- 2026-05-10: Phase 1 complétée (vault, MCPs, GBrain, ACP)
- 2026-05-10: Phase 2 démarrée (ADR-001, workflows, conventions)
