# mason-lsp

Claude Code plugin that auto-detects Neovim Mason-installed LSP servers and makes them available to Claude.

## What this gives Claude Code

Without LSP, Claude navigates your code with grep and file reads. With this plugin, Claude gains real code intelligence:

- **Go to definition** — jump to where a symbol is defined, not just where it's mentioned
- **Find references** — find every usage of a variable, function, or type across the project
- **Hover info** — see types, signatures, and documentation for any symbol
- **Diagnostics** — Claude sees type errors and warnings immediately after each edit and self-corrects

Without this plugin, you'd need to install a separate Claude Code LSP plugin for each language and configure them individually. If you already have LSP servers installed through Mason for your editor, this plugin detects all of them automatically — one plugin instead of many, no duplicate installs, no manual configuration.

## Requirements

- [mason.nvim](https://github.com/williamboman/mason.nvim) with LSP servers installed
- `jq` for JSON processing
- Mason `bin/` directory in your `PATH`:
  ```bash
  export PATH="$HOME/.local/share/nvim/mason/bin:$PATH"
  ```

## Install

See the [root README](../README.md) for marketplace setup. Then:

```
/plugin install mason-lsp@orestesk-plugins
/reload-plugins
```

## Usage

LSP servers sync automatically on session start via the `SessionStart` hook.

To manually re-sync after installing or removing Mason packages:

```
/mason-lsp:sync
```

Then run `/reload-plugins` to apply changes.

## Configuration

**`MASON_DIR`** -- environment variable to override the default Mason data directory (`~/.local/share/nvim/mason`).

**`lang-extensions.json`** -- maps language names from the Mason registry to file extensions. 43 languages supported out of the box. Add entries for new languages.

**`lsp-args.json`** -- overrides the default args (`["--stdio"]`) for LSP binaries that need different startup flags. For example, `gopls` needs `["serve"]` and `lua-language-server` needs `[]`.

## Example output

Generated `.lsp.json` for a setup with basedpyright, lua-language-server, and vtsls:

```json
{
  "python": {
    "command": "basedpyright-langserver",
    "args": ["--stdio"],
    "extensionToLanguage": { ".py": "python", ".pyi": "python" },
    "maxRestarts": 3
  },
  "lua": {
    "command": "lua-language-server",
    "extensionToLanguage": { ".lua": "lua" },
    "maxRestarts": 3
  },
  "javascript": {
    "command": "vtsls",
    "args": ["--stdio"],
    "extensionToLanguage": {
      ".js": "javascript", ".jsx": "javascriptreact",
      ".ts": "typescript", ".tsx": "typescriptreact"
    },
    "maxRestarts": 3
  }
}
```

## Troubleshooting

**`jq` not installed** -- the sync script requires jq. Install with your package manager (`pacman -S jq`, `brew install jq`, etc).

**Mason directory not found** -- set `MASON_DIR` if Mason data lives somewhere other than `~/.local/share/nvim/mason`.

**Binary not in PATH** -- if you see `[WARNING: ... not in PATH]` in the sync output, add Mason's `bin/` to your shell PATH (see Requirements above).

**Unknown language warnings** -- `mason-lsp: unknown language 'Foo'` means `lang-extensions.json` has no mapping for that language. Add an entry if you need it, or ignore the warning.
