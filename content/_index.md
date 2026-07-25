---
title: tsvsheet.obsidian
---

**tsvsheet.obsidian** brings [tsvsheet](https://tsvsheet.com) — a spreadsheet in plain text — into [Obsidian](https://obsidian.md). The plugin embeds the tsvsheet engine (the Go implementation compiled to WebAssembly) and gives it two surfaces in your vault:

- **A `sheet` code block** — a fenced `sheet` block in any note renders as a live, read-only computed grid in Reading view, with the engine's diagnostics listed beneath the grid when there are any.
- **A `.tsvt` file view** — opening a `.tsvt` file shows the computed spreadsheet, with a mode toggle in the view header:
  - **Preview** (the default) renders the _computed_ grid, read-only — what the sheet evaluates to.
  - **Edit** renders the _source_ grid — formulas visible — with every cell editable. Edits recompute through the engine and are saved as canonical `.tsvt` source: comment and shebang lines and the trailing newline are preserved exactly, and computed values are never written back to your file.

- Source: [tsvsheet/tsvsheet.obsidian](https://github.com/tsvsheet/tsvsheet.obsidian)
- Security: [measures and live scan results](security.md)
- Language: [tsvsheet/tsvsheet](https://github.com/tsvsheet/tsvsheet)
- Examples: [tsvsheet.examples](https://github.com/tsvsheet/tsvsheet.examples)

## Install

The plugin is not yet in the community plugin directory. To install manually, copy (or symlink) the repository into your vault's plugin folder and enable **tsvsheet** under **Settings → Community plugins**:

```text
<vault>/.obsidian/plugins/tsvsheet/
  main.js  manifest.json  styles.css  tsvsheet.wasm  wasm_exec.js
```

After updating an installed copy, reload the plugin: toggle it off and on in **Settings → Community plugins**, or reload Obsidian.

## Using it

A `sheet` block in a note:

````text
```sheet
item	qty	price	amount
Widgets	3	4.50	=B2*C2
Total			=sum(D2:D4)
```
````

Cells are TAB-separated; a cell beginning with `=` is a formula over the tsvsheet expression language — A1 references (`B2`, `$B$2`, ranges `D2:D4`), arithmetic, text, logical, date, lookup, and aggregate functions, and the pipe operator. The full language lives at [tsvsheet/tsvsheet](https://github.com/tsvsheet/tsvsheet); worked example sheets are in [tsvsheet.examples](https://github.com/tsvsheet/tsvsheet.examples).

`.tsvt` files in the vault open directly in the spreadsheet view. Use the header pencil / book icon to switch between editing formulas and previewing computed values.
