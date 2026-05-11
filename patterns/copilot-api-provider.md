---
type: pattern
name: GitHub Copilot API as LLM Provider
category: integration
tags: [copilot, github, api, provider, authentication, oauth]
date: 2026-05-11
reuse_count: 1
---

# GitHub Copilot API as LLM Provider

## Problem

On veut utiliser les modèles frontier (GPT-5.4, Claude Opus) via l'API GitHub Copilot (gratuit avec licence enterprise) plutôt que payer les APIs directement. L'authentification utilise un device code OAuth flow non documenté.

## Solution

1. **Obtenir le token** via device code flow GitHub :
```python
# POST https://github.com/login/device/code
# scope: copilot
# → user_code à entrer sur github.com/login/device
# → poll POST https://github.com/login/oauth/access_token
# → access_token (gho_xxx)
```

2. **Échanger contre un token Copilot** :
```python
# GET https://api.github.com/copilot_internal/v2/token
# Authorization: token gho_xxx
# → copilot_token (tid=xxx;exp=xxx;...)
```

3. **Appeler l'API chat/completions** :
```bash
curl https://api.githubcopilot.com/chat/completions \
  -H "Authorization: Bearer <copilot_token>" \
  -H "Copilot-Integration-Id: vscode-chat" \
  -d '{"model": "gpt-5.4", "messages": [...]}'
```

Configuration Hermes :
```yaml
# ~/.hermes/config.yaml
model: "gpt-5.4"
provider: "copilot"
```
```bash
# ~/.hermes/.env
COPILOT_GITHUB_TOKEN=gho_xxxxx
```

## Constraints

- Nécessite une licence GitHub Copilot (Individual, Business, ou Enterprise)
- Token expire ~8h → doit être refresh automatiquement
- Max 128 tools par requête (contrainte Copilot API)
- Certains modèles non disponibles sur /chat/completions (ex: gpt-5.5)
- Le model name doit être envoyé sans préfixe provider (ex: "gpt-5.4" pas "copilot/gpt-5.4")

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Gratuit (inclus dans licence Copilot) | Token refresh périodique |
| Accès GPT-5.4, Claude Opus, Gemini Pro | Rate limiting non documenté |
| Pas de facturation séparée | Max 128 tools (pas de gros toolkit MCP) |
| Même infra que VS Code Copilot | API non officiellement documentée |

## Usage Notes

- Si >128 tools : réduire les MCP servers (retirer Playwright = -23 tools)
- Le provider "copilot" dans Hermes normalise automatiquement les model names
- Pour les comptes EMU (Enterprise Managed Users) : le device code flow utilise le compte enterprise, pas le compte personnel
- Les tokens gho_ sont différents des PAT ghp_ — ne pas confondre

## Related

- [[adr/ADR-001-architecture-ai-command-center]] — Décision d'utiliser Copilot
- [[patterns/mcp-multi-server-architecture]] — Contrainte 128 tools
- [[projects/ai-command-center]] — Projet qui implémente ce pattern

## Timeline

- 2026-05-11: Pattern extrait de la mise en place Copilot API (Phase 3)
