---
type: project
name: hermes-learnings-mcp
status: completed
start_date: 2026-05-11
tags: [mcp, bun, typescript, jsonl, learnings]
---

# Project — hermes-learnings-mcp

## Overview

Serveur MCP autonome en Bun/TypeScript qui expose le store JSONL de learnings Hermes via 6 tools : `log_learning`, `search_learnings`, `list_learnings`, `stats_learnings`, `export_learnings`, `prune_learnings`.

## Stack

- Language: TypeScript
- Runtime: Bun 1.3.12
- Protocol: MCP stdio
- SDK: `@modelcontextprotocol/sdk`
- Storage: JSONL append-only
- Validation: `bun test`, `bun run typecheck`

## Architecture

- [[patterns/bun-typescript-stdio-mcp-server]]
- [[patterns/jsonl-append-only-learning-store]]
- [[patterns/mcp-multi-server-architecture]]

## Key Patterns Used

- [[patterns/bun-typescript-stdio-mcp-server]] — Serveur MCP léger avec outils déclarés inline
- [[patterns/jsonl-append-only-learning-store]] — Store append-only simple et diffable
- [[patterns/mcp-multi-server-architecture]] — Capacité isolée dans un serveur MCP dédié
- [[patterns/explicit-provider-precedence-over-env-autodetect]] — Correction de routage pour respecter le provider explicite

## Tasks

- [[tasks/TASK-002-create-hermes-learnings-mcp-server]]

## Retrospectives

- [[retrospectives/2026-05-11-hermes-learnings-mcp]]

---

## Timeline

- 2026-05-11: Projet créé pour exposer le store Hermes learnings via un serveur MCP Bun/TypeScript
- 2026-05-11: 6 tools MCP implémentés sur transport stdio avec `@modelcontextprotocol/sdk`
- 2026-05-11: Validation OK — `bun test` (11/11) et `bun run typecheck`
- 2026-05-11: Bug de provider routing corrigé — une variable OpenAI-compatible pointant vers Ollama ne doit pas détourner un provider explicite Copilot vers OpenRouter

## Related

- [[projects/ai-command-center]] — Projet parent
- [[tasks/TASK-002-create-hermes-learnings-mcp-server]] — Implémentation
- [[retrospectives/2026-05-11-hermes-learnings-mcp]] — Retro associée
- [[patterns/bun-typescript-stdio-mcp-server]] — Pattern extrait
- [[patterns/explicit-provider-precedence-over-env-autodetect]] — Pattern extrait
