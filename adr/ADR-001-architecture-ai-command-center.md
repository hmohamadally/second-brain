---
type: adr
id: ADR-001
status: accepted
date: 2026-05-10
tags: [architecture, hermes, copilot, obsidian, fondation]
---

# ADR-001 — Architecture AI Engineering Command Center

## Context

Besoin d'un environnement d'ingénierie logicielle augmenté par IA, avec mémoire persistante, capitalisation de connaissances et amélioration cumulative. Le système doit fonctionner sans inférence locale coûteuse et rester compatible avec un environnement entreprise/ESN.

## Decision

Architecture à 3 couches avec séparation stricte des responsabilités :

| Couche | Outil | Responsabilité |
|--------|-------|----------------|
| Orchestration | Hermes Agent | Planning, task decomposition, workflows, memory, automation |
| Génération | GitHub Copilot | Code, refactor, debugging, implémentation multi-fichiers |
| Connaissance | Obsidian + GBrain | Mémoire long terme, knowledge graph, ADRs, patterns |

Communication inter-couches :
- Hermes ↔ Vault : MCP filesystem (`~/second-brain/`)
- Hermes ↔ GBrain : MCP stdio (30 tools, hybrid search)
- Hermes ↔ GitHub : MCP GitHub (26 tools)
- Hermes → Copilot : Task Protocol (fichiers `tasks/TASK-xxx.md`)

## Alternatives

| Option | Pros | Cons |
|--------|------|------|
| Tout-en-un Claude/GPT | Simple, puissant | Pas de mémoire persistante, coût API, pas de capitalisation |
| Agent local seul (Ollama) | Gratuit, privé | Trop faible pour orchestration complexe |
| Architecture choisie | Mémoire cumulative, gratuit (Copilot enterprise), modulaire | Copilot non programmatique, setup initial |

## Consequences

- Hermes ne génère jamais de code complexe directement — il délègue à Copilot via Task Protocol
- Le vault Obsidian est le system of record pour la connaissance humaine
- GBrain est la couche de retrieval pour l'agent
- Tout apprentissage doit être documenté dans le vault (pas éphémère)
- Le modèle local (gemma4) sert de fallback pour orchestration simple ; un modèle frontier est recommandé pour les tâches complexes

## References

- [[projects/ai-command-center]]
- [[workflows/feature-development]]
- [[workflows/debugging]]
