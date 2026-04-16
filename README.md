## Installation

Add the marketplace to Claude Code:

```
/plugin marketplace add OrestesK/claude-plugins
```

Then install individual plugins:

```
/plugin install <plugin-name>@orestesk-plugins
/reload-plugins
```

## Plugins

| Plugin | Description |
|--------|-------------|
| [mason-lsp](./mason-lsp/) | Auto-sync Neovim Mason LSP servers and CLI tools for Claude Code |
| [tree-sitter](./tree-sitter/) | AST-aware code intelligence via ast-grep for Claude Code |

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated

Each plugin may have additional requirements — see its README for details.

## License

[MIT](./LICENSE)
