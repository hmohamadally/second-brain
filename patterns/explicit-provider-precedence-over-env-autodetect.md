---
type: pattern
name: Explicit Provider Precedence Over Env Autodetect
category: architecture
tags: [provider, routing, copilot, openrouter, ollama, env, config]
date: 2026-05-11
reuse_count: 0
---

# Explicit Provider Precedence Over Env Autodetect

## Problem

Quand plusieurs credentials ou endpoints coexistent dans l’environnement, une heuristique d’auto-détection peut router une requête vers le mauvais provider. Exemple concret : une variable `OPENAI_API_KEY` pointant vers Ollama dans `.env` faisait dériver une session configurée pour Copilot vers OpenRouter au lieu d’honorer le provider explicite.

## Solution

Définir une règle stricte de précédence : le provider explicitement choisi dans la config ou la CLI gagne toujours sur les heuristiques basées sur les variables d’environnement. Les variables génériques (`OPENAI_API_KEY`, endpoint compatible OpenAI, proxy Ollama, etc.) ne doivent servir qu’en fallback quand aucun provider n’a été demandé.

Règle opérationnelle :
1. Résoudre d’abord le provider explicite (`copilot`, `anthropic`, `openrouter`, etc.)
2. Charger uniquement les credentials pertinents pour ce provider
3. N’utiliser l’auto-détection par env qu’en absence de provider explicite
4. Ajouter un test de régression dès qu’un nouveau shortcut env est introduit

## Constraints

- Config : fichier YAML + `.env`
- Integrations : providers directs, proxys OpenAI-compatibles, Ollama
- Validation : tests de régression multi-provider

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Empêche les détournements silencieux de routing | La logique de résolution devient plus stricte |
| Rend le comportement explicable à l’utilisateur | Moins de “magie” quand la config est incomplète |
| Réduit les bugs quand plusieurs providers cohabitent | Nécessite plus de tests de précédence |
| Compatible avec des proxys OpenAI locaux (ex: Ollama) | Peut révéler des configs ambigües qui semblaient “marcher” avant |

## Example

```text
Wrong:
- provider: copilot
- OPENAI_API_KEY pointe vers un proxy/local endpoint Ollama
=> la résolution bascule vers openrouter/openai-compatible

Right:
- provider: copilot
- OPENAI_API_KEY pointe vers un proxy/local endpoint Ollama
=> la résolution reste sur copilot ; OPENAI_API_KEY n’est consultée que si aucun provider n’est explicite
```

## Related

- [[patterns/copilot-api-provider]]
- [[patterns/ollama-openai-sdk-proxy]]
- [[projects/hermes-learnings-mcp]]

---

## Timeline

- 2026-05-11: Pattern extrait du correctif de routing Copilot/Ollama/OpenRouter
