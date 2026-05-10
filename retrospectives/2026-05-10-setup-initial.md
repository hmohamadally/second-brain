---
type: retrospective
project: ai-command-center
date: 2026-05-10
duration: ~3h
tags: [setup, hermes, gbrain, mcp, vault, first-run]
---

# Retrospective — AI Command Center / Setup initial

## What Worked

- **GBrain MCP** : L'intégration via `gbrain serve` est seamless — 30 tools disponibles immédiatement
- **Vault structure** : Les 12 dossiers + templates standardisés donnent un cadre clair pour la capitalisation
- **Skills Hermes** : Le format SKILL.md est simple et expressif — un fichier markdown = un workflow complet
- **Cron system** : Plus fiable que le cron intégré Hermes (qui nécessite le gateway) — le crontab système fonctionne indépendamment
- **MCP multi-server** : 4 serveurs cohabitent sans conflit, chacun avec son scope

## Failures

- **`@modelcontextprotocol/server-git`** : Package npm inexistant — temps perdu à debugger. Hermes a ses propres outils git natifs
- **Docker MCP** : Docker Desktop non installé — bloque l'isolation des commandes terminal
- **Modèle local (gemma4)** : Trop faible pour l'orchestration complexe multi-step. Suffisant pour les tâches simples mais inadapté pour le workflow PRD complet
- **Embeddings** : Impossibles sans clé API OpenAI — GBrain fonctionne en keyword-only, pas de recherche sémantique
- **Git identity** : Non configurée initialement — bloque les commits silencieusement

## New Patterns Discovered

- [[patterns/mcp-multi-server-architecture]] — Architecture découplée multi-MCP avec fail-safe

## Reusable Components

- Script `vault-gbrain-sync.sh` — réutilisable pour tout vault git + GBrain ([[snippets/vault-auto-sync]])
- Template de skill Hermes — structure SKILL.md avec frontmatter, workflow, steps
- Structure vault 12 dossiers — applicable à tout projet d'ingénierie IA

## Prompt Effectiveness

- Les prompts de génération de task (/task-gen) produisent des outputs bien structurés quand le brief est précis
- Le prompt de retro (/retrospective) est efficace pour extraire les patterns — la clé est de fournir le contexte git
- Prompt [[prompts/hermes-task-generation]] documenté pour réutilisation

## Recommendations

1. **Priorité 1** : Obtenir une clé API pour un modèle frontier (Anthropic ou OpenRouter) — débloque l'orchestration complexe
2. **Priorité 2** : Clé OpenAI pour embeddings GBrain — débloque la recherche sémantique
3. **Priorité 3** : Installer Docker Desktop — débloque l'isolation et le MCP Docker
4. Commencer à utiliser le workflow réel (brief → task → implem → retro → sync) dès la prochaine feature

## Timeline

- 2026-05-10: Retrospective rédigée après setup complet Phase 1-3
