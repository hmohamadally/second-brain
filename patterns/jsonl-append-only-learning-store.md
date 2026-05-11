---
type: pattern
name: JSONL Append-Only Learning Store
category: architecture
tags: [learning, jsonl, self-improvement, agent, persistence]
date: 2025-07-23
reuse_count: 1
---

# JSONL Append-Only Learning Store

## Problem

Un agent AI accumule des micro-apprentissages au fil des sessions (patterns découverts, erreurs à éviter, préférences utilisateur) mais ces insights sont perdus entre les conversations. Les formats structurés (DB, YAML) sont trop rigides pour des données hétérogènes et fréquemment appendées.

## Solution

Utiliser un fichier JSONL (JSON Lines) append-only par projet. Chaque ligne est un fait atomique auto-contenu avec timestamp, type, clé unique, et score de confiance.

```jsonl
{"ts":"2025-07-23T14:00:00Z","skill":"retrospective","type":"pattern","key":"jwt-refresh-pattern","insight":"Refresh tokens JWT doivent être stockés en httpOnly cookie, pas localStorage","confidence":9,"source":"observed","files":["api/auth.py"]}
{"ts":"2025-07-23T15:00:00Z","skill":"manual","type":"pitfall","key":"sqlite-async","insight":"SQLite + async Python nécessite aiosqlite — sqlite3 stdlib bloque l'event loop","confidence":10,"source":"observed","files":[]}
```

Recherche par `grep`, export par awk/jq, prune par filtrage sur confidence.

## Constraints

- Nécessite un script CLI ou un skill agent pour logger/chercher/exporter
- Pas de déduplication native — convention: clé unique par projet, augmenter confidence plutôt que dupliquer
- Fichiers volumineux (>10K lignes) peuvent ralentir grep — prune régulier nécessaire

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Append-only = pas de corruption, pas de merge conflicts | Pas de requêtes complexes (pas de SQL) |
| Un fichier texte = portable, versionnable, diffable | Déduplication manuelle par convention |
| Chaque ligne indépendante = parsing trivial | Pas de relations entre learnings |
| Pas de schema migration | Taille croissante sans prune |

## Usage Notes

- Utiliser pour des micro-observations factuelles, pas des documents longs
- Le champ `confidence` permet le prune automatique (supprimer < 3)
- Intégrer dans les skills existants (retrospective, pattern-extract) pour capture automatique
- Exporter périodiquement en Markdown pour inclusion dans le vault (queryable via RAG)

## Related

- [[patterns/copilot-api-provider]] — pattern extrait via ce système
- [[patterns/ollama-openai-sdk-proxy]] — pattern extrait via ce système
- [[projects/ai-command-center]] — projet parent

## Timeline

- 2025-07-23: Pattern extrait de l'analyse de gstack v1.31
