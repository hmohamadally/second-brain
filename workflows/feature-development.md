---
type: workflow
name: Feature Development
trigger: manual
tags: [development, feature, standard]
date: 2026-05-10
---

# Workflow — Feature Development

## Trigger

Nouveau ticket, brief produit, ou demande utilisateur à implémenter.

## Steps

### 1. Analyse du besoin

- Actor: hermes
- Input: Brief ou ticket (texte libre)
- Output: Analyse structurée (objectif, contraintes, scope)
- Validation: L'utilisateur confirme la compréhension

### 2. Recherche patterns existants

- Actor: hermes
- Input: Analyse du besoin
- Output: Patterns, ADRs et solutions similaires trouvés dans le vault
- Validation: Au moins une recherche GBrain + filesystem effectuée
- Commandes: `gbrain query "..."`, `gbrain search "..."`

### 3. Architecture & impact

- Actor: hermes
- Input: Analyse + patterns trouvés
- Output: Décision d'architecture, fichiers impactés, dépendances
- Validation: Si nouvelle décision → créer ADR

### 4. Génération task Copilot

- Actor: hermes
- Input: Analyse + architecture + patterns
- Output: `tasks/TASK-xxx.md` selon le Task Protocol
- Validation: Le fichier contient objective, context, constraints, acceptance criteria

### 5. Implémentation

- Actor: copilot (piloté par l'humain)
- Input: Fichier `tasks/TASK-xxx.md` ouvert dans VS Code
- Output: Code implémenté
- Validation: Acceptance criteria remplis

### 6. Tests & validation

- Actor: hermes + humain
- Input: Code implémenté
- Output: Tests passent, lint OK
- Validation: `pytest`, `npm test`, ou équivalent

### 7. Documentation & capitalisation

- Actor: hermes
- Input: Feature terminée
- Output: Mise à jour vault (patterns, learnings, retrospective si significatif)
- Validation: Au moins un fichier vault mis à jour

## Success Criteria

- [ ] Task markdown créée
- [ ] Code implémenté et testé
- [ ] Vault enrichi (pattern ou learning)

## Failure Handling

Si une étape échoue :

1. Documenter le blocage dans `debugging/`
2. Chercher solutions similaires : `gbrain query "erreur ..."`
3. Si résolu → documenter dans le même fichier debugging
4. Si non résolu → escalader à l'humain

## Related

- [[adr/ADR-001-architecture-ai-command-center]]
- [[workflows/debugging]]
