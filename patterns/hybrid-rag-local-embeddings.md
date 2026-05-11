---
type: pattern
name: Hybrid RAG with Local Embeddings
category: integration
tags: [rag, embeddings, ollama, gbrain, search, vector]
date: 2025-07-23
reuse_count: 1
---

# Hybrid RAG with Local Embeddings

## Problem

On veut un système RAG (Retrieval-Augmented Generation) sur un vault de documents sans dépendre d'APIs cloud payantes pour les embeddings. Les solutions cloud (OpenAI, Cohere) coûtent cher à l'échelle et posent des problèmes de confidentialité pour du code propriétaire.

## Solution

Utiliser **Ollama** comme serveur d'embeddings local avec un modèle léger (nomic-embed-text, 768 dimensions), couplé à un knowledge graph (GBrain/PGLite) qui combine recherche keyword + sémantique via Reciprocal Rank Fusion (RRF).

```yaml
# Configuration MCP — injecter les env vars d'embedding dans le serveur GBrain
mcp_servers:
  gbrain:
    command: "~/.bun/bin/gbrain"
    args: ["serve"]
    env:
      OPENAI_API_KEY: "ollama"                          # Fake key (Ollama n'en a pas besoin)
      OPENAI_BASE_URL: "http://localhost:11434/v1"      # Endpoint compatible OpenAI SDK
      GBRAIN_EMBEDDING_MODEL: "nomic-embed-text"        # Modèle local léger
      GBRAIN_EMBEDDING_DIMENSIONS: "768"                # Dimensions du modèle
```

Le trick clé : Ollama expose un endpoint `/v1/embeddings` compatible avec le SDK OpenAI. N'importe quel outil qui utilise le SDK OpenAI pour les embeddings peut pointer vers Ollama sans modification de code.

Architecture :
```
Document → GBrain import → chunking → PGLite (text + metadata)
                                            ↓
Query → keyword search (ts_rank) ──────→ RRF merge → ranked results
      → vector search (cosine sim) ────↗
                                    ↑
                          Ollama (nomic-embed-text)
```

## Constraints

- Ollama doit tourner en permanence (`ollama serve`)
- nomic-embed-text a une limite de 8192 tokens par chunk — les documents très longs échouent
- 768 dimensions = moins précis que text-embedding-3-large (1536d) mais suffisant pour <1000 documents
- Premier embedding est lent (~5 min pour 2000 chunks) mais incrémental ensuite (--stale)

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Gratuit, illimité, local | Moins précis que les modèles cloud |
| Confidentialité totale | Nécessite GPU/CPU pour Ollama |
| Compatible OpenAI SDK (zero code change) | nomic-embed-text = 8K context max |
| Hybrid search (keyword + vector) | Latence plus élevée que cloud embeddings |

## Usage Notes

- Toujours patcher les dimensions du schema DB pour matcher le modèle (768 vs 1536)
- Utiliser `--all` pour le premier embedding, `--stale` pour les suivants
- Les fichiers >8K tokens échouent silencieusement — acceptable pour les docs de référence
- Benchmark local : top-4 fiable sur 10 requêtes de test (40% perfect, 40% partial)

## Related

- [[patterns/ollama-openai-sdk-proxy]] — pattern sous-jacent utilisé
- [[patterns/mcp-multi-server-architecture]] — architecture globale des serveurs MCP
- [[projects/ai-command-center]] — projet parent

## Timeline

- 2025-07-23: Pattern formalisé après benchmark de retrieval (4/10 perfect)
