---
type: task
id: TASK-001
status: done
priority: high
project: ai-command-center
assignee: copilot
date: 2026-05-10
tags: [setup, vault, gbrain, mcp, hermes]
---

# TASK-001 — Setup AI Engineering Command Center

## Objectif

Mettre en place l'environnement complet AI Engineering Command Center : Hermes Agent + GBrain + Vault Obsidian + MCP servers + Skills d'automatisation.

## Contexte

Premier déploiement de l'environnement sur macOS Apple Silicon. Stack locale avec Ollama pour le LLM, GBrain pour le knowledge graph, et 4 serveurs MCP.

## Étapes d'implémentation

1. [x] Installer Hermes Agent v0.8.0 (Python 3.11, venv, uv)
2. [x] Configurer Ollama + gemma4:e4b comme LLM local
3. [x] Installer GBrain v0.7.0 (Bun, PGLite)
4. [x] Créer le vault `~/second-brain/` (12 dossiers, templates, conventions)
5. [x] Configurer 4 MCP servers (gbrain, filesystem, github, playwright)
6. [x] Créer 3 skills (/task-gen, /retrospective, /vault-sync)
7. [x] Installer cron vault-GBrain sync (toutes les 4h)
8. [x] Configurer VS Code ACP
9. [x] Push repos GitHub (Hermes + second-brain)

## Critères d'acceptation

- [x] `hermes` démarre sans erreur
- [x] `hermes mcp list` → 4 serveurs enabled
- [x] Skills scan → 3 skills détectés
- [x] `gbrain stats` → pages > 0
- [x] `crontab -l` → entrée sync présente
- [x] Repos pushés sur GitHub

## Existing Patterns

- [[patterns/mcp-multi-server-architecture]] — Architecture MCP multi-serveur

## Result

Setup complet réalisé le 2026-05-10. 93 tools MCP disponibles, 16 pages dans GBrain, 3 skills opérationnels, cron actif.

## Timeline

- 2026-05-10: Task créée et exécutée — setup complet en une session
