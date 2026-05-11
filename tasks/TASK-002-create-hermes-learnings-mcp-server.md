---
type: task
id: TASK-002
status: todo
priority: high
project: hermes-learnings-mcp
assignee: copilot
date: 2026-05-11
tags: [mcp, bun, typescript, jsonl, learnings]
---

# TASK-002 — Create hermes-learnings-mcp server

## Objective

Créer un serveur MCP `hermes-learnings-mcp` en TypeScript/Bun qui expose un système de learnings JSONL via 6 outils MCP : `log_learning`, `search_learnings`, `list_learnings`, `stats_learnings`, `export_learnings`, `prune_learnings`.

## Context

Le système AI Engineering Command Center s’appuie sur Hermes pour l’orchestration, Copilot pour l’implémentation, et le vault/GBrain pour la mémoire long terme. Un pattern existe déjà pour stocker des micro-apprentissages dans un fichier JSONL append-only, et un autre pattern recommande de découpler chaque capacité sous forme de serveur MCP indépendant. Le dépôt cible doit être créé dans `~/Projects/hermes-learnings-mcp/` et, au moment de la génération de cette task, ce dossier n’existe pas encore.

## Constraints

- Framework: Bun + TypeScript + `@modelcontextprotocol/sdk`
- Architecture: Serveur MCP autonome, transport stdio, stockage JSONL append-only pour les learnings
- Tests: Vérification minimale du build/typecheck, tests unitaires ou d’intégration Bun pour les opérations de store et pour l’enregistrement des 6 tools
- Conventions: Respecter `[[patterns/jsonl-append-only-learning-store]]`, `[[patterns/mcp-multi-server-architecture]]` et `[[adr/ADR-001-architecture-ai-command-center]]`

## Existing Patterns

- [[patterns/jsonl-append-only-learning-store]]
- [[patterns/mcp-multi-server-architecture]]
- [[adr/ADR-001-architecture-ai-command-center]]

## Acceptance Criteria

- [ ] Le dépôt `~/Projects/hermes-learnings-mcp/` contient un projet Bun/TypeScript exécutable qui démarre un serveur MCP sur stdio avec `@modelcontextprotocol/sdk`
- [ ] Les 6 outils MCP (`log_learning`, `search_learnings`, `list_learnings`, `stats_learnings`, `export_learnings`, `prune_learnings`) sont implémentés avec schémas d’entrée/sortie cohérents et comportements documentés
- [ ] Le store JSONL supporte l’append, la recherche filtrée, le listing paginé, les statistiques agrégées, l’export lisible et le prune selon des critères explicites, avec validations automatisées
- [ ] Un README explique le format JSONL, la configuration du chemin de stockage, l’exécution locale avec Bun, et l’intégration du serveur dans une config MCP Hermes

## Files Impacted

```text
~/Projects/hermes-learnings-mcp/
  package.json
  bun.lockb
  tsconfig.json
  README.md
  src/
    index.ts
    server.ts
    schema.ts
    tools/
      log-learning.ts
      search-learnings.ts
      list-learnings.ts
      stats-learnings.ts
      export-learnings.ts
      prune-learnings.ts
    store/
      jsonl-store.ts
      types.ts
      filters.ts
  tests/
    store.test.ts
    tools.test.ts
```

## Implementation Notes

Scaffolder le repo avec Bun et garder la surface API simple : un module store dédié pour lire/écrire le JSONL, un module par tool MCP, et une composition centrale du serveur. Définir un schéma stable pour un learning (ex. id, ts, category/type, key, insight/content, confidence, source, tags, files, metadata). Prévoir un chemin de stockage configurable via variable d’environnement ou argument, avec valeur par défaut raisonnable documentée. `search_learnings` doit supporter au minimum texte libre + filtres simples (type/tag/source/confidence min). `export_learnings` peut produire du JSON ou du Markdown lisible. `prune_learnings` doit être sûr : dry-run par défaut ou option de preview avant suppression effective. Ajouter des exemples d’utilisation et une section “Hermes MCP config” dans le README.

## Validation

```bash
cd ~/Projects/hermes-learnings-mcp
bun install
bun run typecheck
bun test
bun run start
```

## Post-Completion

- [ ] Mettre à jour mémoire vault
- [ ] Créer retrospective si significatif
- [ ] Extraire patterns réutilisables
