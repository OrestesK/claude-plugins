---
name: code-intelligence
description: Navigate and understand code using tree-sitter AST tools. Use when searching for symbols, understanding file structure, finding code patterns, refactoring patterns, or getting codebase overviews. Prefer over grep and LSP for structural code queries.
---

## Tree-sitter-first code intelligence

Use the tiered approach for code understanding:

**Tier 1 — Tree-sitter (fast, always accurate, prefer by default):**
- `search_symbols` — find functions, classes, methods by name
- `document_symbols` — list all symbols in a file
- `symbol_definition` — get full source of one symbol without reading the whole file
- `pattern_search` — structural code search with AST patterns
- `pattern_replace` — automated structural code transformations (dry-run by default)
- `codebase_overview` — languages, file counts, symbol totals
- `codebase_map` — directory tree with symbol counts per file

**Tier 2 — LSP (semantic precision, may be stale after edits):**
- Go to definition — resolve imports and cross-file symbols
- Find references — all usages across the project
- Hover — type info and documentation
- Diagnostics — type errors and warnings (may lag ~2s after edits)

**When to use which:**
- Finding a function by name → `search_symbols` (not grep)
- Understanding a file → `document_symbols` (not reading the whole file)
- Reading one function → `symbol_definition` (not Read on 500-line file)
- Finding a code pattern → `pattern_search` (not grep)
- Refactoring a pattern → `pattern_replace` (not manual Edit)
- Orienting in new codebase → `codebase_overview` then `codebase_map`
- Resolving an import → LSP go-to-definition
- Finding all callers → LSP find-references

**Pattern syntax (ast-grep):**
- `$NAME` — matches a single AST node
- `$$$` — matches zero or more nodes
- Patterns look like real code: `console.log($ARG)`, `def $NAME($$$):`, `fn $NAME($$$) { $$$ }`

**Note:** Claude Code's LSP tool does not support rename or completions.
