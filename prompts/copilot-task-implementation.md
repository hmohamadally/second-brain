---
type: prompt
target: copilot
category: generation
effectiveness: 4
tags: [copilot, task-protocol, generation, standard]
date: 2026-05-10
---

# Prompt — Task Protocol Implementation

## Context d'utilisation

Quand on veut que Copilot implémente une feature à partir d'un Task Protocol markdown.

## Prompt

```text
I need you to implement the following task. Read it carefully, respect all constraints, and implement exactly what's described.

## Task File
[paste or reference tasks/TASK-xxx.md]

## Instructions
1. Follow the constraints strictly
2. Implement all acceptance criteria
3. Only modify the files listed in "Files Impacted"
4. If an existing pattern is referenced, follow it exactly
5. Write tests for every public function
6. Use conventional commits for any git operations

Start implementation now. Show me the file changes one by one.
```

## Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `TASK-xxx.md` | Le fichier task protocol à implémenter | `tasks/TASK-042-jwt-auth.md` |

## Expected Output

Copilot génère le code fichier par fichier, avec explications pour chaque changement.

## Effectiveness Notes

- Score: 4/5
- Fonctionne bien avec : Claude Sonnet, GPT-5
- Ne fonctionne pas avec : Modèles < 30B (perd le contexte des contraintes)
- Amélioration : Inclure des exemples de code du projet pour le style

## Variations

### V1 — Avec contexte architecture

```text
I need you to implement the following task. 

Architecture context:
- [paste relevant ADR summary]
- [paste relevant pattern]

Task:
[paste task]

Constraints are non-negotiable. Implement all acceptance criteria.
```

### V2 — Debug/fix uniquement

```text
Fix the following issue. The task file describes the problem and expected behavior.

[paste task]

Investigation steps so far:
[paste what hermes found]

Fix it. Show me the minimal change needed.
```
