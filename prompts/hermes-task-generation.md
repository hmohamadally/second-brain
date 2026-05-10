---
type: prompt
target: hermes
category: generation
effectiveness: 4
tags: [task-generation, hermes, vault, automation]
date: 2026-05-10
---

# Prompt — Génération de Task Protocol depuis brief

## Context d'utilisation

Quand un brief produit arrive (feature, bugfix, refactor), ce prompt structure la demande en Task Protocol exploitable par Copilot.

## Prompt

```text
À partir du brief suivant, génère un Task Protocol complet.

## Brief
{{brief}}

## Instructions
1. Cherche dans le vault (patterns/, adr/) des solutions similaires avec GBrain
2. Détermine le prochain numéro TASK-xxx dans tasks/
3. Génère le fichier avec :
   - Frontmatter YAML (type, id, status, priority, project, assignee, date, tags)
   - Objectif clair en 1-2 phrases
   - Contexte technique (stack, architecture)
   - Étapes d'implémentation numérotées
   - Critères d'acceptation testables
   - Références aux patterns existants si pertinents
4. Commit le fichier dans le vault

Assigne à "copilot" par défaut. Utilise le format ~/second-brain/tasks/TASK-{NNN}-{slug}.md.
```

## Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `{{brief}}` | Description libre de la feature/tâche | "Ajouter un endpoint REST /api/health avec vérification DB" |

## Expected Output

Fichier `TASK-xxx-slug.md` dans le vault avec frontmatter, objectif, étapes, critères d'acceptation, et références patterns.

## Lessons Learned

- Toujours chercher dans GBrain AVANT de générer — évite de réinventer des patterns existants
- Le slug dans le nom de fichier aide à retrouver la task sans ouvrir le fichier
- "assignee: copilot" signale que c'est prêt pour implémentation via Copilot

## Timeline

- 2026-05-10: Prompt créé à partir du skill /task-gen
