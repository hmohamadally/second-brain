---
type: snippet
name: Cron Sync Script
language: bash
tags: [cron, git, gbrain, sync, automation]
date: 2026-05-10
---

# Snippet — Vault Auto-Sync (Git + GBrain)

## Usage

Script crontab pour synchroniser automatiquement un vault git vers GitHub + GBrain.
Détecte les changements, commit, push, et réindexe.

## Code

```bash
#!/bin/bash
# vault-gbrain-sync.sh — Auto sync vault → GitHub + GBrain
# Crontab: 0 */4 * * * ~/.hermes/scripts/vault-gbrain-sync.sh

VAULT="$HOME/second-brain"
GBRAIN="$HOME/.bun/bin/gbrain"
LOG="$HOME/.hermes/logs/vault-sync.log"

mkdir -p "$(dirname "$LOG")"

{
  echo "=== $(date '+%Y-%m-%d %H:%M:%S') vault-gbrain-sync ==="
  cd "$VAULT" || { echo "ERROR: vault not found"; exit 1; }

  # Skip if no changes
  if git diff --quiet HEAD && [ -z "$(git status --porcelain)" ]; then
    echo "No changes detected, skipping."
    exit 0
  fi

  # Commit + push
  git add -A
  git commit -m "docs: auto vault sync — $(date '+%Y-%m-%d %H:%M')"
  git push origin main 2>&1

  # Reimport into GBrain
  if command -v "$GBRAIN" &>/dev/null; then
    "$GBRAIN" import "$VAULT" --no-embed 2>&1
    echo "GBrain reimport done."
  else
    echo "WARN: gbrain not found, skipping reimport."
  fi

  echo "=== sync complete ==="
} >> "$LOG" 2>&1
```

## Installation

```bash
# Copier le script
mkdir -p ~/.hermes/scripts
cp vault-gbrain-sync.sh ~/.hermes/scripts/
chmod +x ~/.hermes/scripts/vault-gbrain-sync.sh

# Installer le crontab (toutes les 4h)
(crontab -l 2>/dev/null; echo "0 */4 * * * $HOME/.hermes/scripts/vault-gbrain-sync.sh") | crontab -
```

## Notes

- `--no-embed` car les embeddings nécessitent une clé OpenAI
- Les logs s'accumulent dans `~/.hermes/logs/vault-sync.log` — penser à rotate
- Le script est idempotent — safe à relancer manuellement
