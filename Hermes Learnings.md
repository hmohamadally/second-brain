# Hermes Learnings Export

> 17 learnings

## Architecture

### mcp-128-tools-limit

Le provider Copilot a une limite de 128 tools par requête MCP — retirer Playwright (94 tools) pour rester sous la limite

- **Confidence:** 10
- **Source:** observed
- **Files:** ~/.hermes/config.yaml
- **Date:** 2025-07-23T13:00:00Z

### harry2mf-pipeline-stages

harry2mf uses a clean 4-stage pipeline: lexer (regex tokenization) → parser (statement_parser.py, recursive descent) → AST (dataclass nodes) → mapper (snowflake_generator.py, visitor pattern). Each stage is isolated but there's no intermediate validation or error recovery between stages.

- **Confidence:** 0.9
- **Source:** code-review
- **Tags:** harry2mf, architecture, compiler
- **Files:** harry2mf/parser/statement_parser.py, harry2mf/mapper/snowflake_generator.py, harry2mf/ast/nodes.py
- **Date:** 2026-05-11T15:12:26.149Z

### harry2mf-reporter-empty

The reporter module (harry2mf/reporter/) is completely empty — no validation report, no transpilation quality metrics, no coverage analysis. This is a planned but unimplemented feature that would be critical for production use (tracking which DSL constructs are handled vs marked UNCERTAIN).

- **Confidence:** 0.9
- **Source:** code-review
- **Tags:** harry2mf, gap, reporting
- **Files:** harry2mf/reporter/
- **Date:** 2026-05-11T15:12:26.150Z

## Operational

### gbrain-embed-stale-noop

gbrain embed --stale ne détecte pas les chunks nouvellement importés — utiliser --all à la place

- **Confidence:** 9
- **Source:** observed
- **Date:** 2025-07-23T12:00:00Z

### gbrain-code-indexing

gbrain sync --repo ~/Hermes indexe le code source comme pages concept — 568 fichiers = 568 pages dans la DB

- **Confidence:** 9
- **Source:** observed
- **Date:** 2025-07-23T15:00:00Z

### harry2mf-no-git-no-ci

Harry-CC repo has no git initialization, no CI/CD, no pre-commit hooks. With 83 tests passing and 2964 LOC, it's mature enough for version control. Priority: git init + GitHub remote + basic pytest CI action.

- **Confidence:** 0.95
- **Source:** code-review
- **Tags:** harry2mf, devops, maturity
- **Files:** pyproject.toml
- **Date:** 2026-05-11T15:12:26.150Z

## Pattern

### ollama-openai-proxy

Ollama expose un endpoint /v1/embeddings compatible OpenAI SDK — permet de réutiliser tout client OpenAI sans modification

- **Confidence:** 9
- **Source:** observed
- **Files:** ~/.bun/install/global/node_modules/gbrain/src/core/embedding.ts
- **Date:** 2025-07-23T10:00:00Z

### copilot-api-provider

GitHub Copilot API utilise un device code flow avec token gho_ et base URL api.githubcopilot.com — compatible OpenAI SDK

- **Confidence:** 9
- **Source:** observed
- **Files:** ~/.hermes/config.yaml
- **Date:** 2025-07-23T10:05:00Z

### gstack-learn-system

gstack utilise un fichier JSONL append-only par projet pour capturer les micro-learnings — architecture simple et efficace pour l'auto-amélioration agent

- **Confidence:** 8
- **Source:** observed
- **Date:** 2025-07-23T14:00:00Z

### gstack-context-save

gstack sauvegarde le contexte session dans un WIP commit avec bloc structuré [hermes-context] — permet la reprise après crash

- **Confidence:** 8
- **Source:** observed
- **Date:** 2025-07-23T14:30:00Z

### harry2mf-cte-staging-pattern

The SQL generator emits Snowflake SQL using a CTE-per-stage pattern (WITH stage_0_X AS (...), stage_1_Y AS (...) SELECT * FROM last_stage). This is idiomatic Snowflake and enables query plan optimization. Each Harry 2 MF LECRAN maps to one CTE stage.

- **Confidence:** 0.9
- **Source:** code-review
- **Tags:** harry2mf, sql, snowflake, pattern
- **Files:** harry2mf/mapper/snowflake_generator.py
- **Date:** 2026-05-11T15:12:26.150Z

## Pitfall

### gbrain-dry-run-imports

gbrain sync --dry-run importe quand même les pages dans la DB — le flag dry-run n'est pas implémenté correctement

- **Confidence:** 10
- **Source:** observed
- **Date:** 2025-07-23T11:00:00Z

### nomic-context-length

nomic-embed-text a une limite de 8192 tokens — les fichiers .md volumineux (release notes, docs packages) échouent silencieusement

- **Confidence:** 10
- **Source:** observed
- **Date:** 2025-07-23T11:30:00Z

### harry2mf-lrelation-self-join

The LRELATION handler in snowflake_generator.py generates self-joins (LEFT JOIN T ON T.col = T.col) instead of joining the related table. The join target table must be resolved from the relation definition, not reuse the source table name.

- **Confidence:** 0.95
- **Source:** code-review
- **Tags:** harry2mf, sql-generation, bug
- **Files:** harry2mf/mapper/snowflake_generator.py
- **Date:** 2026-05-11T15:12:26.149Z

### harry2mf-corpus-insufficient

Only 2 corpus scripts exist (DELCOUAR, DELCOU01) for a tool that claims learn/learn-batch capabilities. The knowledge YAML is hand-maintained. The learning pipeline cannot generalize without 20+ diverse corpus examples covering all DSL constructs.

- **Confidence:** 0.85
- **Source:** code-review
- **Tags:** harry2mf, data, coverage
- **Files:** corpus/, harry2mf/knowledge/
- **Date:** 2026-05-11T15:12:26.150Z

### harry2mf-esel-nipro-unhandled

ESEL and NIPRO statements in Harry 2 MF are marked [UNCERTAIN] in generated SQL. The parser recognizes them but the mapper cannot generate correct Snowflake equivalents. These appear to be multi-value selection/filter constructs that need semantic documentation from domain experts.

- **Confidence:** 0.8
- **Source:** code-review
- **Tags:** harry2mf, dsl-coverage, uncertain
- **Files:** harry2mf/mapper/snowflake_generator.py
- **Date:** 2026-05-11T15:12:26.150Z

## Tool

### zsh-wikilinks-escaping

zsh interprète [[wikilinks]] comme des conditionnels — utiliser heredoc Python ou printf pour écrire du contenu avec des wikilinks

- **Confidence:** 8
- **Source:** observed
- **Date:** 2025-07-23T12:30:00Z
