---
type: workflow
name: Debugging
trigger: manual
tags: [debugging, incident, standard]
date: 2026-05-10
---

# Workflow — Debugging

## Trigger

Erreur, bug, échec de test, ou comportement inattendu.

## Steps

### 1. Analyse des symptômes

- Actor: hermes
- Input: Message d'erreur, logs, description du problème
- Output: Symptômes structurés (quoi, quand, où, reproduisible ?)
- Validation: Problème clairement identifié

### 2. Recherche incidents similaires

- Actor: hermes
- Input: Symptômes
- Output: Incidents passés, patterns de résolution
- Commandes: `gbrain query "erreur ..."`, `gbrain search "..."`
- Validation: Au moins une recherche effectuée

### 3. Hypothèses

- Actor: hermes
- Input: Symptômes + historique
- Output: Liste d'hypothèses classées par probabilité
- Validation: Au moins 2 hypothèses

### 4. Stratégie de résolution

- Actor: hermes
- Input: Hypothèses
- Output: Plan de debug (commandes à exécuter, fichiers à inspecter)
- Validation: Plan actionnable

### 5. Investigation

- Actor: copilot + hermes
- Input: Stratégie
- Output: Cause racine identifiée
- Validation: Hypothèse confirmée par les preuves

### 6. Fix

- Actor: copilot
- Input: Cause racine
- Output: Correction implémentée
- Validation: Tests passent, bug ne se reproduit plus

### 7. Documentation

- Actor: hermes
- Input: Incident résolu
- Output: Fichier `debugging/incident-xxx.md`
- Validation: Root cause, resolution, prevention documentées

## Success Criteria

- [ ] Cause racine identifiée
- [ ] Fix implémenté et testé
- [ ] Incident documenté dans le vault

## Failure Handling

1. Si pas de résolution après 30 min → changer d'hypothèse
2. Si toutes les hypothèses épuisées → réduire le scope (reproduire minimal)
3. Documenter même les tentatives échouées (pour capitalisation)

## Related

- [[adr/ADR-001-architecture-ai-command-center]]
- [[workflows/feature-development]]
