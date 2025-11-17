# Configuring `nvim-treesitter`

`nvim-treesitter` is typically configured in Lua via the module `nvim-treesitter.configs`.

## Key Options in `nvim-treesitter.configs.setup`

When you call:

```lua
require("nvim-treesitter.configs").setup({
  … 
})
```

you are passing a Lua table of configuration options. These govern both parser installation and the behavior of various *modules*. Below are the most important (and common) options, and what they do:

1. **`ensure_installed`**

   * Specifies which Tree-Sitter parsers to install.
   * Accepts a list of parser names (e.g. `{"lua", "python", "javascript"}`) or special values like `"all"`.
   * These are the parsers that the plugin ensures are present (if you run installation commands).

2. **`sync_install`**

   * A boolean flag. When `true`, installation of parsers listed in `ensure_installed` happens *synchronously*.
   * Useful if you want to block startup until parsers are ready (but may slow down startup).

3. **`auto_install`**

   * Boolean. If set to `true`, `nvim-treesitter` will automatically install missing parsers when you enter a buffer whose language parser is not yet installed.
   * Note: this assumes you have a working Tree-Sitter CLI or the ability to compile/install parsers on demand.

4. **`ignore_install`**

   * A list of parser names to *exclude* from installation (even if you said `"all"` in `ensure_installed`).
   * Handy when you want most parsers but know some are unnecessary or too heavy for your use case.

5. **`parser_install_dir`**

   * A path (as a string) where parsers will be installed, other than the default.
   * If you use a custom directory, you must also prepend that path to Neovim’s `runtimepath`, so Neovim knows where to look for those parsers.

---

## Notes on Modules

* Module-specific options available include: `highlight`, `indent`, `incremental_selection`, `textobjects`, `folds`, `playground`, etc.
* Modules are **not enabled by default**. You must explicitly activate them in the `setup` call.
* Each module encapsulates a specific feature (highlighting, moving between syntax nodes, text-objects, etc.).
* You can also **define your own modules** if you want custom behavior: `nvim-treesitter` allows registration of modules with `define_modules`, providing `attach` and `detach` behavior for buffers.

**Note:** These modules are covered in different files within this directory.

---
