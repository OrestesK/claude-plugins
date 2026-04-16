# tree-sitter

Claude Code plugin that provides AST-aware code intelligence via [ast-grep](https://ast-grep.github.io/).

## What this gives Claude Code

Without this plugin, Claude navigates your code with grep and reads entire files. With this plugin, Claude gets structural code intelligence:

- **Symbol search** — find functions, classes, methods by name with fuzzy matching, not text grep
- **File outlines** — list all symbols in a file without reading 500 lines
- **Symbol extraction** — get just one function's source code, not the whole file
- **Pattern search** — structural code search that matches AST, not text (won't match comments or strings)
- **Pattern replace** — automated structural refactoring with dry-run preview
- **Codebase overview** — languages, file counts, symbol totals at a glance
- **Codebase map** — directory tree with symbol counts per file

Tree-sitter is fast, always accurate, and never stale. It complements LSP: use tree-sitter for structural queries, LSP for semantic precision (go-to-definition, find-references, hover, diagnostics).

## Requirements

- **ast-grep** installed:
  ```bash
  pacman -S ast-grep    # Arch
  brew install ast-grep  # macOS
  cargo install ast-grep # Cargo
  ```
- **Node.js** 18+

## Install

See the [root README](../README.md) for marketplace setup. Then:

```
/plugin install tree-sitter@orestesk-plugins
/reload-plugins
```

## MCP Tools

| Tool | Description | Example |
|------|-------------|---------|
| `search_symbols` | Find symbols by name with fuzzy matching | `search_symbols(query: "handle", path: "./src")` |
| `document_symbols` | List all symbols in a file | `document_symbols(file_path: "src/server.ts")` |
| `symbol_definition` | Get full source of one symbol | `symbol_definition(file_path: "src/server.ts", symbol_name: "handleRequest")` |
| `pattern_search` | Structural code search with AST patterns | `pattern_search(pattern: "console.log($$$)", language: "typescript")` |
| `pattern_replace` | Structural code transformation (dry-run default) | `pattern_replace(pattern: "console.log($A)", replacement: "logger.info($A)", language: "typescript")` |
| `codebase_overview` | Languages, file counts, symbol totals | `codebase_overview(path: ".")` |
| `codebase_map` | Directory tree with symbol counts | `codebase_map(path: "./src", depth: 2)` |

## Tree-sitter vs LSP

| Need | Tool | Speed | Stale? |
|------|------|-------|--------|
| Find symbols by name | search_symbols (tree-sitter) | Fast | Never |
| File outline | document_symbols (tree-sitter) | Fast | Never |
| Read one function | symbol_definition (tree-sitter) | Fast | Never |
| Structural pattern search | pattern_search (tree-sitter) | Fast | Never |
| Structural refactoring | pattern_replace (tree-sitter) | Fast | Never |
| Find all references | LSP findReferences | Slower | Sometimes |
| Go to definition | LSP goToDefinition | Slower | Sometimes |
| Type info / hover | LSP hover | Slower | Sometimes |
| Diagnostics after edit | LSP diagnostics | Slow | Often stale |

**Note:** Claude Code's LSP tool does not support rename or completions.

## Supported languages

JavaScript, TypeScript, Python, Lua, Rust, Go, Java, C, C++, Ruby, Kotlin, Swift

## Pattern syntax

ast-grep patterns look like real code with wildcards:

- `$NAME` — matches a single AST node
- `$$$` — matches zero or more nodes

```
# Find all console.log calls
pattern_search(pattern: "console.log($$$)", language: "javascript")

# Find async functions
pattern_search(pattern: "async function $NAME($$$) { $$$ }", language: "typescript")

# Replace var with const
pattern_replace(pattern: "var $N = $V", replacement: "const $N = $V", language: "javascript")
```

See [ast-grep pattern syntax](https://ast-grep.github.io/guide/pattern-syntax.html) for full documentation.

## Troubleshooting

**ast-grep not installed** — the server requires ast-grep. Install with your package manager (see Requirements above).

**No symbols found** — the file's language might not be in the supported list. Check the extension is mapped in the server.

**Pattern syntax errors** — patterns must be valid code fragments for the target language. See [ast-grep docs](https://ast-grep.github.io/guide/pattern-syntax.html) for syntax help.

**Timeout on large codebases** — symbol scans have a 15s timeout (30s for overview/map). For very large repos, narrow searches with the `path` parameter.
