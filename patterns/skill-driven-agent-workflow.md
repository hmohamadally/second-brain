---
type: pattern
name: Skill-Driven Agent Workflow
category: architecture
tags: [agent, skills, automation, hermes, prompt-engineering]
date: 2025-07-23
reuse_count: 4
---

# Skill-Driven Agent Workflow

## Problem

Un agent AI généraliste peut tout faire mais fait tout moyennement. Les workflows récurrents (générer un task protocol, faire une retro, syncer le vault) nécessitent des instructions précises et reproductibles. Hardcoder ces workflows dans le code de l'agent est rigide et non extensible.

## Solution

Externaliser les workflows comme des **skill files** — des fichiers Markdown avec frontmatter YAML qui décrivent un workflow step-by-step. L'agent les charge dynamiquement via des slash commands.

```
~/.hermes/skills/engineering/
├── task-gen/SKILL.md           # /task-gen — génère un task protocol
├── retrospective/SKILL.md      # /retrospective — capitalise les apprentissages
├── vault-sync/SKILL.md         # /vault-sync — commit + push + reimport
├── pattern-extract/SKILL.md    # /pattern-extract — extrait des patterns
├── context-save/SKILL.md       # /context-save — persistence de session
├── learn/SKILL.md              # /learn — apprentissage continu JSONL
└── context-inject/SKILL.md     # /context-inject — charge le contexte projet
```

Chaque SKILL.md contient :
1. **Frontmatter** — metadata (name, description, tags, prerequisites)
2. **Objectif** — ce que le skill fait
3. **Workflow** — steps numérotés avec commandes et templates
4. **Règles** — contraintes et bonnes pratiques

Le contenu du skill est injecté comme **user message** (pas system prompt) pour préserver le prompt caching.

## Constraints

- Le skill est un prompt, pas du code — l'agent l'interprète, ne l'exécute pas mécaniquement
- Les skills longs (>2000 tokens) consomment du contexte
- Pas de variables/conditionnels — c'est du texte statique injecté
- L'agent peut dévier du workflow si le contexte l'exige

## Tradeoffs

| Avantage | Inconvénient |
|----------|--------------|
| Extensible sans code (juste ajouter un .md) | L'agent peut mal interpréter le workflow |
| Versionnable (git), diffable, reviewable | Pas de garantie d'exécution exacte |
| Réutilisable entre projets et agents | Consomme du contexte LLM |
| Compose avec les outils MCP existants | Debugging indirect (pas de stack trace) |

## Usage Notes

- Un skill = un workflow atomique. Ne pas mélanger plusieurs responsabilités
- Inclure des exemples concrets dans le workflow (l'agent apprend par l'exemple)
- Tester le skill sur 2-3 cas avant de le considérer stable
- Le frontmatter `prerequisites.commands` permet de vérifier les dépendances
- Les skills s'intègrent entre eux via des appels croisés (/retrospective appelle /learn log)

## Related

- [[patterns/jsonl-append-only-learning-store]] — skill /learn utilise ce pattern
- [[patterns/wip-context-save-commit]] — skill /context-save utilise ce pattern
- [[projects/ai-command-center]] — projet parent

## Timeline

- 2025-07-23: Pattern formalisé à partir de 7 skills opérationnels
