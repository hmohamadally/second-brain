# Hermes — Learnings

> Auto-generated from `~/.hermes/learnings/hermes.jsonl` on 2026-05-11

## Patterns

- **ollama-openai-proxy** (confidence: 9): Ollama expose un endpoint /v1/embeddings compatible OpenAI SDK — permet de réutiliser tout client OpenAI sans modification
- **copilot-api-provider** (confidence: 9): GitHub Copilot API utilise un device code flow avec token gho_ et base URL api.githubcopilot.com — compatible OpenAI SDK
- **gstack-learn-system** (confidence: 8): gstack utilise un fichier JSONL append-only par projet pour capturer les micro-learnings — architecture simple et efficace pour l'auto-amélioration agent
- **gstack-context-save** (confidence: 8): gstack sauvegarde le contexte session dans un WIP commit avec bloc structuré [hermes-context] — permet la reprise après crash

## Pitfalls

- **gbrain-dry-run-imports** (confidence: 10): gbrain sync --dry-run importe quand même les pages dans la DB — le flag dry-run n'est pas implémenté correctement
- **nomic-context-length** (confidence: 10): nomic-embed-text a une limite de 8192 tokens — les fichiers .md volumineux (release notes, docs packages) échouent silencieusement

## Operationals

- **gbrain-embed-stale-noop** (confidence: 9): gbrain embed --stale ne détecte pas les chunks nouvellement importés — utiliser --all à la place
- **gbrain-code-indexing** (confidence: 9): gbrain sync --repo ~/Hermes indexe le code source comme pages concept — 568 fichiers = 568 pages dans la DB

## Architectures

- **mcp-128-tools-limit** (confidence: 10): Le provider Copilot a une limite de 128 tools par requête MCP — retirer Playwright (94 tools) pour rester sous la limite

## Tools

- **zsh-wikilinks-escaping** (confidence: 8): zsh interprète [[wikilinks]] comme des conditionnels — utiliser heredoc Python ou printf pour écrire du contenu avec des wikilinks

