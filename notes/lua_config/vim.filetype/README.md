# `vim.filetype`

## Background and Purpose

In Neovim, accurate filetype detection is essential: it determines syntax highlighting, indentation rules, filetype‑specific plugins, and more. Historically, filetype detection was done using Vimscript (via `filetype.vim`), which relies on many autocommands triggered on `BufRead` and `BufNewFile` events.

However, Neovim introduced a faster, more flexible Lua-based mechanism via the `vim.filetype` module. This new approach uses a single autocommand and Lua logic under the hood, leading to better performance (reduced startup cost) and more expressive detection.

Neovim’s `vim.filetype` module provides several key functions:

1. **`vim.filetype.add(...)`**; register custom filetype mappings
2. **`vim.filetype.match(...)`**; perform filetype detection manually
3. **`vim.filetype.get_option(filetype, option)`**; query default option values for a filetype

---

## Using `vim.filetype.add`

This is the primary API to extend or override filetype detection.

### Signature and Components

```lua
vim.filetype.add({
  extension = { … },   -- map file extensions to filetypes
  filename = { … },    -- map full filenames (or tail names) to filetypes
  pattern = { … },     -- map Lua-style path or filename patterns to filetypes
})
```

* **`extension`**: a table mapping file extensions (without dot) to either a string (filetype) or a function.
* **`filename`**: map exact filenames (or relative tails) to filetypes.
* **`pattern`**: map Lua string‑patterns (not regex) to filetypes; these patterns can include capture groups, which are passed to the callback.
* For functions, the signature is `(path, bufnr, captures…) → filetype[, post_fn]`, where `post_fn` is an optional function that is called (with the buffer number) after the filetype is set, allowing you to modify buffer state (e.g., set buffer-local variables).
* Patterns can have an optional **priority** to resolve conflicts (higher priority wins).

### Example: Basic Extension Mapping

```lua
vim.filetype.add({
  extension = {
    foo = "fooscript",
    bar = function(path, bufnr)
      -- custom logic (maybe read first lines, or decide dynamically)
      return "barfile"
    end,
  },
})
```

This mapping will ensure that:

* Any file ending with `.foo` gets the filetype `fooscript`.
* Any file ending with `.bar` will run the function, which returns `barfile` to be used as its filetype.

### Example: Pattern-Based Mapping

Suppose you have special YAML files in a `.github/workflows/` directory that you want to mark as a custom filetype `yaml.ghaction`:

```lua
vim.filetype.add({
  pattern = {
    [".*/%.github/workflows/.*%.yml"] = "yaml.ghaction",
  },
})
```

With this, any file whose full path matches that Lua pattern will be detected as `yaml.ghaction`. This allows you to distinguish it from general `yaml` files for e.g. linting or syntax rules.

---

## Manual Detection with `vim.filetype.match`

You can manually query what filetype Neovim would assign given certain parameters. This is useful for testing or for code that wants to act on a file's determined type.

```lua
local filetype, post_fn = vim.filetype.match({
  buf = bufnr,               -- use an existing buffer
  filename = "foo.txt",      -- override its filename
  -- or
  contents = { "#!/bin/sh" } -- specify file contents
})
```

* The function returns a **string** (the detected filetype) and optionally a **buffer‑state function** (`post_fn`).
* If `post_fn` is returned, you should call it with the buffer number to apply any additional buffer-local configuration.

---

## Querying Options with `vim.filetype.get_option`

Once a filetype is detected, Neovim applies certain default options, loading ftplugins and triggering `FileType` autocommands. You may want to programmatically inspect what a given filetype’s default configuration would be.

```lua
local comment_str = vim.filetype.get_option("vim", "commentstring")
```

This returns the `commentstring` option as it would be for a new buffer of filetype `vim`.

Note: the result is **cached**, so repeated calls do not retrigger ftplugin or autocmd logic.

---

## Enabling Lua-Based Filetype Detection

* By default, legacy Vimscript-based filetype detection is active. Neovim offers `filetype.lua` as an *opt-in* mechanism.

* To enable it, set the following global before initialization in your config:

  ```vim
  let g:do_filetype_lua = 1
  ```

* If you disable the older `filetype.vim` logic, you risk missing detection for some rare or less common filetypes. Many users enable both for continuity.

---
