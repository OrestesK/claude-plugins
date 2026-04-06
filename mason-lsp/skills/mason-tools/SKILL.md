---
name: mason-tools
description: Format, lint, or type-check code using Neovim Mason tools. Use when asked to format files, fix lint errors, run a linter, or check types.
---

CLI tools installed via Neovim Mason are available on PATH at `~/.local/share/nvim/mason/bin/`.

Run `ls ~/.local/share/nvim/mason/bin/` to discover available tools, then use them directly. Common examples:
- `stylua <file>` — format Lua
- `ruff check <file>` / `ruff format <file>` — lint/format Python
- `prettier --write <file>` — format JS/TS/CSS/HTML/JSON/YAML
