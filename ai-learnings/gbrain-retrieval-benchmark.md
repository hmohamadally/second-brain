---
type: ai-learning
topic: GBrain Retrieval Quality
date: 2026-05-11
tags: [gbrain, retrieval, embeddings, benchmark, nomic-embed-text]
---

# Learning — GBrain Hybrid Search Quality (nomic-embed-text 768d)

## Discovery

Benchmark de 10 requêtes sémantiques sur un vault de 22 pages avec embeddings nomic-embed-text (768 dimensions, Ollama local).

## Benchmark Results

| # | Query | Expected Top | Actual Top | Hit? |
|---|-------|-------------|------------|------|
| 1 | configurer MCP servers | patterns/mcp-multi-server | tasks/task-001 (#3: pattern) | ~Partial |
| 2 | workflow debugging bug | workflows/debugging | workflows/debugging | ✅ Perfect |
| 3 | architecture 3 couches | adr/adr-001 | adr/adr-001 | ✅ Perfect |
| 4 | script cron sync vault | snippets/vault-auto-sync | retrospectives (#3: snippet) | ~Partial |
| 5 | prompt copilot task | prompts/copilot-task | prompts/copilot-task | ✅ Perfect |
| 6 | échecs setup initial | retrospectives/setup | workflows/ai-engineering (#4: retro) | ❌ Miss |
| 7 | générer task depuis brief | prompts/hermes-task-gen | workflows/feature-dev | ~Partial |
| 8 | étapes feature development | workflows/feature-dev | workflows/_template | ❌ Miss |
| 9 | conventions nommage | conventions-linking | conventions-linking | ✅ Perfect |
| 10 | stack technique | projects/ai-command-center | workflows/ai-engineering (#3: project) | ~Partial |

**Score: 4/10 perfect, 4/10 partial (top 3-4), 2/10 miss**

## Analysis

- Scores très proches (0.03-0.05 range) → faible différenciation sémantique sur un petit corpus
- Le composant keyword (tsvector) domine avec RRF (Reciprocal Rank Fusion)
- Les templates `_template.md` polluent le ranking (mots génériques)
- Les pages longues/denses (retro, task-001) attirent trop de requêtes variées
- nomic-embed-text est correct pour le français/anglais mixte mais pas optimal

## Key Insight

Avec 22 pages et embeddings locaux : **la qualité est suffisante pour le cas d'usage** (l'agent a toujours la bonne page dans le top 4). Pour améliorer :

1. Exclure les _template.md de l'index (bruit inutile)
2. Augmenter le corpus (plus de pages = meilleure différenciation)
3. Ajouter des metadata queries (type-aware search)

## Impact on Practice

- Utiliser `gbrain query` pour le retrieval contextuel → résultat top-4 fiable
- Combiner avec `gbrain search` (keyword) pour les requêtes précises
- Ne pas dépendre du premier résultat seul — toujours prendre les 3-4 premiers

## Related

- [[projects/ai-command-center]] — Projet parent
- [[adr/ADR-001-architecture-ai-command-center]] — Architecture retrieval layer
- [[patterns/mcp-multi-server-architecture]] — GBrain MCP server config
