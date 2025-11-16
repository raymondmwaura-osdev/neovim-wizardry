# `nvim-treesitter`

1. **What Is Tree-sitter?**
   Tree‑sitter is a modern parsing library designed to build *incremental parse trees* of source code. It provides a way to construct a concrete syntax tree (CST) for a buffer in real time, enabling more accurate, structured understanding of code. Neovim integrates Tree‑sitter to leverage these parsed trees.

2. **Why Use `nvim-treesitter`?**

   * `nvim-treesitter` is a Neovim plugin that provides a high-level abstraction over Tree‑sitter integration, making it easier to install parsers and use Tree‑sitter features.
   * It enables **enhanced syntax highlighting**, beyond what regex-based Vim syntax can offer, because highlighting can be based on the actual syntax tree.
   * Additional modules provide **incremental selection**, **indentation**, and **folding**, all driven by the syntax tree, making code navigation and editing more semantic-aware.
   * It also provides a foundation for more advanced tools (e.g., custom queries, text-objects) by exposing Tree-sitter query interfaces.

---

## Prerequisites

Before installing `nvim-treesitter`, ensure you have:

* **Neovim** (version compatible with Tree‑sitter; recent versions recommended).
* A **C compiler** in your system path (e.g., `gcc`, `clang`). This is necessary because the Tree‑sitter parsers are built from source.
* Utilities for downloading and extracting (e.g., `curl` + `tar`) or `git`, depending on how parsers are fetched.

---

## Installation

Here is how to install `nvim-treesitter` in Neovim, depending on your plugin manager:

1. **Using `vim-plug`:**
   In your `init.vim`:

   ```vim
   Plug 'nvim-treesitter/nvim-treesitter', {'do': ':TSUpdate'}
   lua require'nvim-treesitter.configs'.setup{ highlight = { enable = true } }
   ```

2. **Using `packer.nvim`:**
   In your `packer` configuration:

   ```lua
   use {
     'nvim-treesitter/nvim-treesitter',
     run = ':TSUpdate',
     config = function()
       require("nvim-treesitter.configs").setup {
         ensure_installed = { "c", "lua", "python" },  -- example languages
         sync_install = false,
         highlight = { enable = true },
         indent = { enable = true },
       }
     end
   }
   ```

3. **Using `lazy.nvim`:**

   ```lua
   require("lazy").setup({
     {
       "nvim-treesitter/nvim-treesitter",
       build = ":TSUpdate",
       config = function()
         require("nvim-treesitter.configs").setup {
           ensure_installed = { "javascript", "html", "lua" },
           highlight = { enable = true },
           indent = { enable = true },
         }
       end
     }
   })
   ```

4. **Windows Specific Notes:**

   * A C compiler is mandatory (e.g., `gcc`, `clang`, or `zig`).
   * On Windows, it is recommended to configure Treesitter to use `curl` rather than `git` for fetching parsers (because of deprecation of git-based downloads).

---

## Configuration and Basic Usage

Once installed, you typically configure `nvim-treesitter` in Lua via the module `nvim-treesitter.configs`.

Here are the basic steps:

1. **Setup (in Lua):**

   ```lua
   require("nvim-treesitter.configs").setup {
     ensure_installed = { "lua", "python", "javascript" },  -- languages you want
     sync_install = false,  -- install parsers asynchronously
     auto_install = true,   -- install missing parsers when entering buffer
     highlight = {
       enable = true,        -- enable syntax highlighting
       additional_vim_regex_highlighting = false,
     },
     indent = {
       enable = true,        -- enable treesitter-based indentation
     },
     incremental_selection = {
       enable = true,
       keymaps = {
         init_selection = "gnn",
         node_incremental = "grn",
         scope_incremental = "grc",
         node_decremental = "grm",
       },
     },
     -- You may enable more modules like folding, textobjects, etc.
   }
   ```

2. **Installing Parsers:**

   * In Neovim, run `:TSInstall <language>` (e.g., `:TSInstall python`) to compile and install a parser for a given language.
   * Use `:TSInstallInfo` to list available languages and see which parsers are installed.
   * To update installed parsers, use `:TSUpdate` (or `:TSUpdate all`).

3. **Using Incremental Selection:**
   With the configuration above, you can:

   * Press **`gnn`** to initiate selection at the node under the cursor.
   * Then use **`grn`** to expand the selected node.
   * **`grc`** to expand to a larger scope.
   * **`grm`** to shrink the node selection.
     This lets you semantically select expressions, statements, blocks, etc.

4. **Folding with Treesitter:**

   * Enable folding in Neovim by setting:
     ```vim
     set foldmethod=expr
     set foldexpr=v:lua.vim.treesitter.foldexpr()
     ```
   * This uses the syntax tree to compute fold boundaries.

5. **Custom Queries (Advanced):**

   * Treesitter uses *query files* (written in “s‑expression” syntax) to define captures that modules use (for example, for highlighting).
   * You can override or extend queries via `vim.treesitter.query.set()`.
   * You can also write predicates in query files (e.g., `eq?`, `match?`) to filter matches.

6. **Health Check:**

   * Use `:checkhealth` in Neovim to ensure that Tree‑sitter and its parsers are installed correctly.
   * Specifically, `:checkhealth treesitter` helps identify broken or missing parsers.

---

## Limitations and Considerations

* **Experimental Features:** Some Tree-sitter modules (e.g., indent) are still marked as experimental.
* **Compatibility Constraints:** Parsers must match the versions expected by `nvim-treesitter`. When the plugin is updated, it's important to run `:TSUpdate` so parsers are recompiled to compatible versions.
* **Platform Constraints:** On Windows, building parsers requires a functional C compiler, and misconfiguration can lead to build failures.
* **Runtime Cost:** Although Tree-sitter is efficient, enabling very large numbers of parsers or modules may have memory or startup cost implications, so configure parsers judiciously.

---
