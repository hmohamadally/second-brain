---
type: pattern
name: Bun TypeScript Stdio MCP Server
category: architecture
tags: [bun, typescript, mcp, stdio, sdk, jsonl]
date: 2026-05-11
reuse_count: 0
---

# Bun TypeScript Stdio MCP Server

## Problem

On veut exposer une capacité locale simple (store JSONL, wrappers, automation légère) à un agent via MCP sans surconstruire une API HTTP, une base de données ou une architecture multi-processus complexe.

## Solution

Implémenter un serveur MCP minimal en Bun + TypeScript avec `@modelcontextprotocol/sdk` et le transport stdio. Déclarer les tools directement dans un point d’entrée unique, et déléguer la logique métier à une couche de store pure et testable.

```ts
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new McpServer({ name: "hermes-learnings-mcp", version: "1.0.0" });

server.tool("stats_learnings", "Get aggregate statistics", {}, async () => {
  return { content: [{ type: "text", text: JSON.stringify(store.stats(), null, 2) }] };
});

await server.connect(new StdioServerTransport());
```

Pattern recommandé :
- `src/index.ts` = déclaration des tools + boot MCP
- `src/store/*` = logique pure (lecture/écriture, filtres, export, prune)
- `tests/*` = validation du store et des cas critiques
- README = config Hermes MCP prête à copier

## Constraints

- Runtime : Bun
- Language : TypeScript strict
- Dependencies : `@modelcontextprotocol/sdk`, `zod`
- Protocol : MCP stdio

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Très faible friction de setup | Débogage via stdio moins confortable qu’une API HTTP |
| Surface API claire et petite | Point d’entrée unique peut grossir si trop de tools |
| Couche store testable indépendamment | Pas d’observabilité native sans logs additionnels |
| Intégration Hermes simple (`command: bun`) | Dépend du runtime Bun sur la machine hôte |

## Example

```text
Project layout:
- src/index.ts
- src/store/jsonl-store.ts
- tests/store.test.ts
- README.md
```

## Related

- [[patterns/jsonl-append-only-learning-store]]
- [[patterns/mcp-multi-server-architecture]]
- [[projects/hermes-learnings-mcp]]

---

## Timeline

- 2026-05-11: Pattern extrait de l’implémentation de `hermes-learnings-mcp`
