---
type: pattern
name: MCP Multi-Server Architecture
category: architecture
tags: [mcp, hermes, integration, tooling]
date: 2026-05-10
reuse_count: 0
---

# MCP Multi-Server Architecture

## Problem

Un agent AI a besoin d'accéder à des systèmes hétérogènes (knowledge graph, filesystem, APIs, navigateur) sans couplage fort. Chaque système a son propre protocole, ses propres dépendances, et peut tomber indépendamment.

## Solution

Utiliser le **Model Context Protocol (MCP)** pour exposer chaque système comme un serveur de tools indépendant. L'agent découvre les outils disponibles au démarrage et les invoque de manière uniforme.

```yaml
# ~/.hermes/config.yaml
mcp_servers:
  gbrain:           # Knowledge graph — 30 tools
    command: "gbrain"
    args: ["serve"]
  filesystem:       # Accès fichiers — 14 tools
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-filesystem", "~/vault"]
  github:           # API GitHub — 26 tools
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
  playwright:       # Browser automation — 23 tools
    command: "npx"
    args: ["-y", "@playwright/mcp@latest"]
```

Chaque serveur est un processus indépendant avec un timeout configurable. Si un serveur tombe, les autres continuent de fonctionner.

## Constraints

- Runtime : Node.js (npx) ou binaire natif
- Protocol : MCP (stdin/stdout JSON-RPC)
- Agent : Hermes Agent (ou tout client MCP)

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Découplage total entre systèmes | Startup plus lent (N processus à lancer) |
| Ajout d'un serveur = 3 lignes YAML | Debugging cross-server plus complexe |
| Chaque serveur évolue indépendamment | Overhead mémoire (N processus Node) |
| Fail-safe : un crash n'affecte pas les autres | Pas de transactions cross-server |

## Usage Notes

- Tester chaque serveur individuellement avant intégration (`hermes mcp list`)
- Mettre un `timeout: 120` pour éviter les blocages au démarrage
- Les env vars sensibles (tokens) vont dans la config MCP, pas dans `.env` global
- Vérifier la compatibilité npx : certains packages MCP sont dépréciés (ex: `@modelcontextprotocol/server-git` n'existe pas)

## Timeline

- 2026-05-10: Pattern extrait du setup AI Engineering Command Center (4 serveurs, 93 tools)
