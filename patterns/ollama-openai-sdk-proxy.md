---
type: pattern
name: Ollama OpenAI SDK Proxy
category: integration
tags: [ollama, openai, embeddings, proxy, compatibility]
date: 2026-05-11
reuse_count: 1
---

# Ollama OpenAI SDK Proxy

## Problem

Un outil (GBrain, LangChain, etc.) utilise le SDK OpenAI pour les embeddings mais on veut utiliser un modèle local (Ollama) sans clé API payante. Le SDK attend `OPENAI_API_KEY` et `OPENAI_BASE_URL` pointant vers api.openai.com.

## Solution

Ollama expose une API compatible OpenAI sur `localhost:11434/v1`. On redirige le SDK vers Ollama via les variables d'environnement :

```bash
export OPENAI_API_KEY=ollama          # Valeur bidon mais requise par le SDK
export OPENAI_BASE_URL=http://localhost:11434/v1
export GBRAIN_EMBEDDING_MODEL=nomic-embed-text
export GBRAIN_EMBEDDING_DIMENSIONS=768
```

Le SDK OpenAI fonctionne sans modification — il appelle `/v1/embeddings` sur Ollama au lieu d'OpenAI.

## Constraints

- Ollama doit tourner (`ollama serve`)
- Le modèle d'embedding doit être tiré (`ollama pull nomic-embed-text`)
- Les dimensions doivent matcher le schéma DB (768 pour nomic, pas 1536)
- Certains outils hardcodent le modèle/dimensions → patch source nécessaire

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Gratuit, local, privé | Plus lent que l'API OpenAI cloud |
| Pas de rate limiting | Qualité embeddings inférieure aux modèles commerciaux |
| Fonctionne offline | Nécessite RAM GPU (nomic ≈ 300MB VRAM) |
| Compatible tout SDK OpenAI | Dimension réduite (768 vs 1536/3072) |

## Usage Notes

- Fonctionne avec : GBrain, LangChain, LlamaIndex, tout client OpenAI SDK
- Ne PAS utiliser si la qualité des embeddings est critique (RAG production)
- Vérifier la compatibilité `/v1/embeddings` avec `curl http://localhost:11434/v1/embeddings -d '{"model":"nomic-embed-text","input":"test"}'`
- Si l'outil ne lit pas les env vars, patcher la source (voir GBrain embedding.ts)

## Related

- [[adr/ADR-001-architecture-ai-command-center]] — Architecture qui utilise ce pattern
- [[ai-learnings/gbrain-retrieval-benchmark]] — Benchmark des résultats

## Timeline

- 2026-05-11: Pattern extrait du setup GBrain embeddings (Phase 4)
