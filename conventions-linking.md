---
type: conventions
tags: [meta, linking, standards]
date: 2026-05-10
---

# Conventions de Linking

## Règles de nommage des fichiers

- Tout en minuscules, kebab-case : `feature-development.md`
- Préfixe par catégorie pour les ADRs : `ADR-001-titre.md`
- Préfixe par catégorie pour les tasks : `TASK-001-titre.md`
- Pas d'espaces, pas d'accents dans les noms de fichiers

## Wikilinks

Utiliser les `[[wikilinks]]` Obsidian pour créer des connexions :

```markdown
[[adr/ADR-001-architecture-ai-command-center]]
[[patterns/jwt-auth-pattern]]
[[workflows/feature-development]]
```

## Règles de linking

| Quand | Lier vers |
|-------|-----------|
| Nouvelle décision d'architecture | `adr/` correspondant |
| Réutilisation d'une solution | `patterns/` correspondant |
| Erreur résolue | `debugging/` correspondant |
| Fin de feature | `retrospectives/` si significatif |
| Nouveau projet | `projects/` fiche projet |

## Back-links obligatoires

Chaque fichier doit avoir une section `## Related` en bas avec les liens vers :
- Les fichiers qui l'ont motivé
- Les fichiers qui en dépendent

## Tags

Tags YAML en frontmatter, toujours en minuscules, en anglais :

```yaml
tags: [backend, auth, fastapi, pattern]
```

## Types de pages

| Type | Dossier | Description |
|------|---------|-------------|
| adr | `adr/` | Décision d'architecture |
| pattern | `patterns/` | Solution réutilisable |
| workflow | `workflows/` | Processus standardisé |
| debugging | `debugging/` | Incident et résolution |
| retrospective | `retrospectives/` | Analyse post-feature |
| prompt | `prompts/` | Prompt efficace documenté |
| project | `projects/` | Fiche projet |
| task | `tasks/` | Tâche active |
| ai-learning | `ai-learnings/` | Apprentissage IA |
| snippet | `snippets/` | Code réutilisable |
