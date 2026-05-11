---
type: workflow
name: AI Engineering Setup
trigger: manual
tags: [setup, hermes, gbrain, mcp, vault, onboarding]
date: 2026-05-10
---

# Workflow — AI Engineering Environment Setup

## Trigger

Premier setup d'un environnement AI Engineering Command Center sur une nouvelle machine, ou reconstruction après crash.

## Steps

### 1. Installer les runtimes

- Actor: human
- Input: macOS clean
- Output: Python 3.11, Node.js, Bun, Ollama, uv, Git
- Validation: `python3.11 --version && node --version && bun --version && ollama --version`

### 2. Cloner et installer Hermes

- Actor: human
- Input: Repo GitHub
- Output: Hermes Agent fonctionnel
- Validation: `hermes --version`
- Commandes:
  ```bash
  git clone https://github.com/hmohamadally/Hermes.git && cd Hermes
  python3.11 -m venv venv && source venv/bin/activate
  uv pip install -e ".[all,dev]"
  hermes setup
  ```

### 3. Configurer le LLM

- Actor: human/hermes
- Input: Choix du provider (Ollama local ou API cloud)
- Output: `~/.hermes/config.yaml` avec model + provider
- Validation: `hermes` → réponse cohérente
- Note: gemma4:e4b pour local, Claude/GPT-4 pour orchestration complexe

### 4. Installer GBrain

- Actor: human
- Input: Bun installé
- Output: GBrain opérationnel
- Validation: `gbrain stats`
- Commandes:
  ```bash
  bun add -g github:garrytan/gbrain
  gbrain init
  ```

### 5. Créer le vault Obsidian

- Actor: hermes
- Input: Brief structure vault
- Output: `~/second-brain/` avec 12 dossiers + templates + conventions
- Validation: `find ~/second-brain -name "_template.md" | wc -l` → 12
- Note: Initialiser git, push sur GitHub

### 6. Configurer les MCP servers

- Actor: hermes
- Input: GBrain + vault + GitHub token
- Output: 4 serveurs MCP dans config.yaml (93 tools)
- Validation: `hermes mcp list` → 4 serveurs enabled

### 7. Créer les skills d'automatisation

- Actor: hermes
- Input: Spécifications skills
- Output: 3 skills dans `~/.hermes/skills/engineering/`
- Validation: Scan → `/task-gen`, `/retrospective`, `/vault-sync` détectés

### 8. Installer le cron sync

- Actor: hermes
- Input: Script de sync
- Output: Crontab `0 */4 * * *` + script + dossier logs
- Validation: `crontab -l` → entrée présente

### 9. Importer le vault dans GBrain

- Actor: hermes
- Input: Vault peuplé
- Output: Pages indexées dans GBrain
- Validation: `gbrain stats` → pages > 0

### 10. Validation end-to-end

- Actor: human
- Input: Brief test
- Output: Cycle complet brief → task → implem → retro → sync
- Validation: Fichier task + retro dans le vault, GBrain mis à jour

## Duration estimée

~2-3 heures pour un setup complet (hors installation Docker).

## Known Issues

- `@modelcontextprotocol/server-git` n'existe pas sur npm — Hermes a des outils git natifs
- Docker MCP nécessite Docker Desktop (non inclus dans le setup de base)
- Embeddings GBrain nécessitent une clé API OpenAI séparée

## Timeline

- 2026-05-10: Workflow documenté à partir du setup réel


## Related

- [[projects/ai-command-center]] — Projet qui utilise ce workflow
- [[snippets/vault-auto-sync]] — Script utilisé à l'étape 8
- [[adr/ADR-001-architecture-ai-command-center]] — Architecture qui le motive
