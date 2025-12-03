# `mapleader`

The `mapleader` variable defines the global prefix key (the “leader key”) that substitutes the placeholder `<Leader>` in key-mapping definitions. When you define mappings using `<Leader>…`, Neovim replaces `<Leader>` with the value of `mapleader`. This allows users to create a custom prefix for their custom or plugin-defined shortcuts, minimizing conflicts with built-in commands and improving ergonomics.

Setting `mapleader` is typically done to optimize workflow (by choosing a convenient or easy-to-hit key), to avoid conflicts (especially with default or plugin mappings), or to align keybindings across environments or teammates (UX alignment).

* **Scope**: global (affects all buffers/windows within the Neovim session); the variable belongs to the global namespace (g:).
* **Type**: string (a key sequence), configured via Lua (e.g. `vim.g.mapleader = …`) or Vimscript (e.g. `let mapleader = …`); not a “builtin option,” but a user-definable global variable.
* **Default Value**: backslash (`"\\"`); if `mapleader` is not explicitly defined, `<Leader>` expands to `\`.

* A common convention is to set `mapleader` to a simple, easy-to-press key such as `<Space>` or `,`. For example, many Neovim Lua-based configs begin with: `vim.g.mapleader = ' '` (space) so that `<leader>…` becomes `<Space>…`.
* Plugins that define mappings often use `<Leader>` or `<LocalLeader>` (which maps via `maplocalleader`) to offer default keybindings; but users almost always override `mapleader` to avoid collisions or adopt their preferred modifiers.

---

## Example

```lua
-- in init.lua
vim.g.mapleader = " "

-- later in mapping definitions
vim.keymap.set('n', '<leader>w', '<cmd>write<cr>', { desc = 'Save file' })
vim.keymap.set('n', '<leader>q', '<cmd>quitall<cr>', { desc = 'Quit all' })
```

With this configuration: pressing `Space + w` writes the file; `Space + q` quits all. If instead `mapleader` is left at default, these would be triggered by `\ w` and `\ q`.

---

## Important Notes

* **Timing matters**; `mapleader` must be set *before* any mappings that use `<Leader>` are defined. If you define mappings first, they will use the then-current value (or default) of `mapleader`, and changing `mapleader` later will *not* retroactively update those mappings.
* **mapleader is not an “option”**; it is a global variable. Attempts to set it via option-style APIs (e.g. `vim.opt.mapleader = " "`) will not work. Instead, use `vim.g.mapleader = …`.
* If `mapleader` is undefined (never set), `<Leader>` defaults to `\`.
* For buffer-local mappings, there exists a parallel variable `maplocalleader` (accessed as `g:maplocalleader`); used in conjunction with `<LocalLeader>` when defining buffer-local or filetype-specific mappings.
* Once a mapping is defined with `<Leader>`, it is resolved immediately: changing `mapleader` later does *not* update that mapping’s keybinding.
* Overuse of leader-based mappings can introduce “ambiguous” delays: when a user presses the leader, Neovim may wait for subsequent keys to determine the full mapping. If many leader-based mappings exist or timeouts are long (see `timeoutlen`), this can cause perceptible input lag or interfere with normal typing (especially if leader is set to `<Space>`). Some users adjust `timeoutlen` to mitigate this.

---

## Related Variables / Alternatives

* `maplocalleader`: Global-variable counterpart for local (buffer-local / filetype-local) leader key, used with `<LocalLeader>`. Enables different leader for plugin/filetype-specific shortcuts, reducing collision risk.
* `<Leader>` / `<LocalLeader>`: Special placeholders in mapping commands that expand to the values of `mapleader` and `maplocalleader`, respectively.
* Direct key mappings (without leader): Instead of using a leader-based prefix, one can map keys directly (e.g. `nmap <F5> :make<CR>`), though this increases risk of collision with built-in or plugin mappings.
* Using multi-character or non-standard leader values: Although less common, users can assign any string (single or multiple characters) to `mapleader`; but complexity or ergonomic inconvenience can arise, and readability/consistency tends to suffer.

---
