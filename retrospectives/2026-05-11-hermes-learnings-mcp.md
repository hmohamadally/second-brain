---
type: retrospective
project: hermes-learnings-mcp
date: 2026-05-11
duration: ~2h
tags: [mcp, bun, typescript, jsonl, learnings, provider-routing]
---

# Retrospective — hermes-learnings-mcp / Learnings MCP server

## What Worked

- **Bun + TypeScript + `@modelcontextprotocol/sdk`** a permis de livrer un serveur MCP stdio très léger, sans boilerplate réseau inutile.
- **Le pattern JSONL append-only existant** s’est mappé directement sur une API de store unique (`append`, `search`, `list`, `stats`, `export`, `prune`).
- **Une seule couche store testable** a gardé la logique cohérente entre les 6 tools et a simplifié la validation.
- **README orienté intégration Hermes** a rendu l’usage immédiat avec un snippet `mcp_servers` prêt à copier.
- **Validation rapide avec Bun** : `bun test` (11/11) et `bun run typecheck` ont donné un signal de qualité immédiat.

## Failures

- **Le routing provider était trop permissif** : une variable `OPENAI_API_KEY` pointant vers Ollama dans `.env` détournait une config Copilot vers OpenRouter au lieu d’honorer le provider explicite.
- **Le repo feature n’était pas initialisé en git**, ce qui réduit la traçabilité fine du travail et a limité la collecte automatique de contexte via `git log`.
- **La couverture test se concentre sur le store**, pas encore sur l’enregistrement/contrat des tools MCP eux-mêmes ; le cœur est validé, mais la surface d’intégration mérite un test dédié.

## New Patterns Discovered

- [[patterns/bun-typescript-stdio-mcp-server]]
- [[patterns/explicit-provider-precedence-over-env-autodetect]]

## Reusable Components

- `src/store/jsonl-store.ts` — store réutilisable pour tout backend de learnings JSONL.
- Déclaration centralisée des types de learning (`pattern`, `pitfall`, `operational`, etc.).
- Snippet README de configuration Hermes MCP pour brancher un serveur stdio local.

## Prompt Effectiveness

| Prompt | Result | Score (1-5) |
|--------|--------|-------------|
| Brief feature avec 6 tools nommés + runtime Bun/TypeScript + critères d’acceptation | A cadré immédiatement la surface API et évité la dérive fonctionnelle | 5 |
| Référence aux patterns `jsonl-append-only-learning-store` et `mcp-multi-server-architecture` | A permis de garder un design simple, local et découplé | 5 |
| Signalement précis du bug d’auto-détection OpenAI-compatible qui hijackait Copilot | A rendu le défaut de précédence reproductible et actionnable | 4 |

## Learnings

- Pour une capacité locale simple, un serveur MCP thin-wrapper au-dessus d’un store pur est souvent suffisant ; pas besoin d’ajouter HTTP ou DB trop tôt.
- Les règles de résolution provider doivent être explicites et testées dès que plusieurs credentials/env vars coexistent.
- Le couple Bun + MCP SDK est particulièrement adapté aux petits serveurs outils avec boot rapide et faible surface.

## Action Items

- [ ] Ajouter un test de régression Hermes sur la précédence provider explicite vs auto-détection env
- [ ] Ajouter un test d’intégration sur l’enregistrement des 6 tools MCP
- [ ] Initialiser un dépôt git dédié pour `~/Projects/hermes-learnings-mcp/` si le serveur devient un composant maintenu séparément

## Related

- [[tasks/TASK-002-create-hermes-learnings-mcp-server]]
- [[projects/hermes-learnings-mcp]]
- [[projects/ai-command-center]]
- [[patterns/bun-typescript-stdio-mcp-server]]
- [[patterns/explicit-provider-precedence-over-env-autodetect]]

---

## Timeline

- 2026-05-11: Retrospective créée après implémentation du serveur MCP learnings et correctif de routing provider
