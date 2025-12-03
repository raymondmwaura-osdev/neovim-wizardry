# `vim.keymap.set`

## Overview

`vim.keymap.set` is a Lua-side convenience wrapper for defining key mappings in Neovim. It operates on Neovim’s global or buffer-level key-mapping state. It exists to provide a more ergonomic and Lua-native way of setting mappings (compared to older API calls), and a developer calls it when they want to bind key sequences to commands or Lua functions in a Lua-based config or plugin.

---

## Signature

```lua
vim.keymap.set(mode, lhs, rhs, opts?)
```

---

## Parameters

* **mode** (string or list of strings); Required. Specifies the mode(s) in which the mapping should apply (e.g. `"n"` for normal mode, `"i"` for insert mode, or `{"n","v"}` for normal + visual).
* **lhs** (string); Required. The left-hand side: the key sequence the user will type to trigger the mapping.
* **rhs** (string or function); Required. The right-hand side: either a string (which will be “typed” as if user pressed those keys/commands) or a Lua function (to execute arbitrary logic).
* **opts** (table); Optional. A table of options modifying mapping behavior. Options include:

  * `buffer` (integer or boolean); If given, mapping is buffer-local. `0` or `true` means current buffer.
  * `remap` (boolean, default `false`); Whether the mapping should be recursive (i.e. allow other mappings in its rhs to be followed).
  * `silent` (boolean); Suppress command echo/output.
  * `expr` (boolean); Treat rhs as expression: if `true` and rhs is a function, interpret its return value as keys to execute (with term-codes handled).
  * `desc` (string); A description of the mapping (useful for documentation / which-key-like plugins).
  * Possibly other misc map-arguments that mirror underlying Vim map options.

---

## Returns

* Return type: **nil**.
* On success: returns `nil`.
* On failure: typically raises a runtime error (e.g. invalid mode or invalid arguments); there is no special table structure returned.

---

## Usage

### Example: Basic

```lua
vim.keymap.set('n', '<Leader>w', ':w<CR>')
```

Maps `<Leader>w` in normal mode to save the current buffer.

### Example: Real-world Integration

```lua
-- in init.lua or plugin config
vim.keymap.set({'n', 'v'}, '<Leader>y', '"+y', { desc = "Yank to system clipboard", silent = true })

-- buffer-local mapping inside an autocmd
vim.api.nvim_create_autocmd("FileType", {
  pattern = "lua",
  callback = function()
    vim.keymap.set('n', '<Leader>r', vim.lsp.buf.rename, { buffer = 0, desc = "LSP rename symbol" })
  end
})
```

This shows typical usage: clipboard mapping, and buffer-local LSP mapping using auto-commands.

---

## Errors and Edge Cases

* If `mode` is invalid (not a recognized mode string or list), the call errors.
* If `lhs` is not a valid string, or `rhs` is neither string nor function; error.
* If `opts` contains invalid fields; error.
* For buffer-local mappings: if specifying a non-existent buffer number, behavior is undefined / error.
* Using `expr = true` with a `rhs` string is meaningless (should be function).
* A known footgun: passing the result of calling a function rather than the function itself.

  ```lua
  -- wrong: this calls the function immediately and maps the result
  vim.keymap.set('n', 'x', some_module.do_something())
  -- correct: pass a function
  vim.keymap.set('n', 'x', function() some_module.do_something() end)
  ```

  This mistake can silently succeed (no error) but produce unexpected behavior.
* Mappings may get overridden by other plugins or startup scripts (depending on load order and plugin behavior). For example, some plugin keymaps may override user-defined ones.

---

## Related APIs

* vim.keymap.del; remove a mapping previously defined.
* vim.api.nvim_set_keymap / vim.api.nvim_buf_set_keymap; lower-level API equivalents, more verbose, older style.
* Vimscript mapping commands (:map, :noremap, etc.); the legacy equivalents for mapping in Vim and Neovim.

---

## Testing / Verification

* After defining a mapping, you can run commands like `:map`, `:nmap`, `:verbose map <lhs>` to verify that the mapping is registered.
* For mappings to Lua functions, `:map <lhs>` will show something like `<Lua function>` in the RHS. A `desc` (if provided) will show in mapping listing.
* In plugin tests (Lua testing frameworks), after running your `vim.keymap.set(...)` call, you can simulate keypresses (or call the mapped function directly) and assert side-effects (buffer changes, return values, state).
* For buffer-local mappings — switch to correct buffer and test that mapping only works there.

---
