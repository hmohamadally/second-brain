---
type: pattern
name: WIP Context Save Commit
category: workflow
tags: [git, context, session, persistence, recovery, agent]
date: 2025-07-23
reuse_count: 0
---

# WIP Context Save Commit

## Problem

Un agent AI ou un développeur travaille sur une tâche complexe multi-session. En cas de crash, timeout, ou context switch, l'état mental de la session est perdu : décisions prises, approches tentées et échouées, travail restant. Reprendre coûte du temps et risque de re-tenter des approches déjà invalidées.

## Solution

Créer un commit WIP avec un bloc structuré `[hermes-context]` dans le message de commit. Ce bloc est parseable par l'agent pour reconstruire l'état de la session.

```
WIP: implémentation auth JWT + rate limiting

[hermes-context]
Decisions: JWT RS256 avec rotation des clés, rate limit par IP via Redis
Remaining: tests E2E, documentation OpenAPI, migration DB
Tried: rate limit in-memory (ne survit pas au restart), JWT HS256 (pas assez sécurisé)
Files: api/auth.py, api/middleware/rate_limit.py, tests/test_auth.py
Branch: feature/auth-jwt
Date: 2025-07-23 14:30
[/hermes-context]
```

Restauration :
```bash
git log --oneline --grep="hermes-context" -10
git show <hash> --format="%B" --no-patch
```

## Constraints

- Ne pas `git add -A` aveuglément — seulement les fichiers intentionnels
- Le code dans le WIP commit doit compiler/fonctionner (pas de syntax errors)
- Commit local uniquement — ne pas push automatiquement
- Préfixer par `WIP:` pour permettre le squash ultérieur

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Zero infra (git natif) | Pollue l'historique git si non squashé |
| Parseable par l'agent | Format rigide, pas extensible facilement |
| Fonctionne hors-ligne | Limité à un repo à la fois |
| Bloc structuré = restauration automatique | Nécessite discipline de `WIP:` prefix |

## Usage Notes

- Invoquer avant un context switch ou en fin de session longue
- Le bloc `Tried` est le plus précieux — évite de re-tenter des impasses
- Combiner avec le système de learnings JSONL pour persister les insights au-delà du repo
- Squash les WIP commits avant merge dans main

## Related

- [[patterns/jsonl-append-only-learning-store]] — complément pour les insights cross-projet
- [[projects/ai-command-center]] — projet parent

## Timeline

- 2025-07-23: Pattern extrait de l'analyse de gstack v1.31
