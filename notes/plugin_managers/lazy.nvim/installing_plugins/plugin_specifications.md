## Plugin Specifications

### Repository Identifier (the first element)

**Meaning**: The minimal required value in a plugin spec is the repository identifier (typically `"owner/repo"` for GitHub). This tells lazy.nvim which remote plugin to fetch/install.
**Example**:

```lua
{ "folke/tokyonight.nvim" }
```

---

### `lazy`

**Meaning**: A boolean flag indicating whether the plugin should be loaded *lazily* (i.e., only when required) or loaded at startup. If `true`, loading is deferred until a trigger (event/command/filetype/keys) or when a module is required. If `false`, the plugin is loaded during startup.
**Example**:

```lua
{ "folke/which-key.nvim", lazy = true }
```

---

### `event`

**Meaning**: A string or list of strings (or function returning list) specifying Neovim events that trigger plugin loading. When the named event occurs (e.g., `BufRead`, `InsertEnter`), lazy.nvim will load the plugin.
**Example**:

```lua
{
  "hrsh7th/nvim-cmp",
  event = "InsertEnter"
}
```

---

### `cmd`

**Meaning**: A string or list of strings (or function) specifying commands that will trigger the plugin to load. When the user executes one of those commands (e.g., `:Telescope`), the plugin will be loaded.
**Example**:

```lua
{
  "dstein64/vim-startuptime",
  cmd = "StartupTime"
}
```

---

### `ft`

**Meaning**: A string or list (or function) indicating file‑types for which the plugin should be loaded. When a buffer of that filetype is opened, loading occurs.
**Example**:

```lua
{
  "nvim-neorg/neorg",
  ft = "norg"
}
```

---

### `keys`

**Meaning**: A string or list (or function) defining key mappings that trigger plugin loading. When one of the specified keys is used (in the specified mode), lazy.nvim loads the plugin. The keys can also include a table giving `lhs`, `rhs`, `desc`, `mode`, etc.
**Example**:

```lua
{
  "Wansmer/treesj",
  keys = { { "J", "<cmd>TSJToggle<cr>", desc = "Join Toggle" } }
}
```

---

### `init`

**Meaning**: A function called during startup *before* the plugin loads. Useful for setting global variables (`vim.g.*`) or other pre‐load configuration, especially for Vimscript plugins that rely on globals before loading.
**Example**:

```lua
{
  "dstein64/vim-startuptime",
  init = function()
    vim.g.startuptime_tries = 10
  end
}
```

---

### `opts`

**Meaning**: A table (or function returning a table) of options that will be passed to the plugin’s `setup()` function (or equivalent). This is the preferred way to configure a plugin when it supports Lua‑style setup. If you supply `opts`, lazy.nvim will automatically call `require(MAIN).setup(opts)` unless you override via `config`.
**Example**:

```lua
{
  "nvim-neorg/neorg",
  ft = "norg",
  opts = {
    load = {
      ["core.defaults"] = {}
    }
  }
}
```

---

### `config`

**Meaning**: A function (or boolean `true`) that executes when the plugin is loaded. If `config = true` and `opts` is provided, lazy.nvim uses the default implementation (`require(MAIN).setup(opts)`). If you supply a custom function, it gives you full control of what to do at load-time.
**Example**:

```lua
{
  "folke/tokyonight.nvim",
  lazy = false,
  priority = 1000,
  config = function()
    vim.cmd([[colorscheme tokyonight]])
  end
}
```

---

### `priority`

**Meaning**: A number used to enforce load order when multiple plugins load at startup (i.e., when `lazy = false`). Higher priority values load earlier. This is especially useful for things like colourschemes which should load before other UI plugins to avoid highlight conflicts. Default is 50.
**Example**:

```lua
{
  "folke/tokyonight.nvim",
  lazy = false,
  priority = 1000
}
```

---

### Versioning Fields: `branch`, `tag`, `commit`, `version`, `pin`, `submodules`

**Meaning**: These fields control which version of the plugin to install and how to fetch the repository.

* `branch`: specify the branch name to use.
* `tag`: specify a Git tag.
* `commit`: a Git commit hash.
* `version`: semver string or range (supports full semver).
* `pin`: boolean; when `true`, plugin will not be included in updates.
* `submodules`: boolean; whether to fetch git submodules.
  **Example**:

```lua
{
  "nvim-treesitter/nvim-treesitter",
  tag = "v0.9.0",
  commit = "abcdef123456",
  version = "0.9.x",
  pin = true
}
```

---

### `dependencies`

**Meaning**: A list of plugin names or full plugin specs that must be installed (and possibly loaded) before the parent plugin loads. Useful when one plugin builds upon another or needs it present. Note: In many cases, lazy.nvim’s automatic module loading means you may not need to explicitly declare dependencies.
**Example**:

```lua
{
  "hrsh7th/nvim-cmp",
  event = "InsertEnter",
  dependencies = { "hrsh7th/cmp-nvim-lsp", "hrsh7th/cmp-buffer" },
  config = function()
    require("cmp").setup({ ... })
  end
}
```

---

### `enabled`

**Meaning**: A boolean (or function that returns boolean) specifying whether the plugin should be included at all in the final spec. If `false`, the plugin is skipped (not installed or loaded).
**Example**:

```lua
{ "folke/trouble.nvim", enabled = false }
```

---

### `cond`

**Meaning**: A boolean (or function) used to conditionally load the plugin, but without uninstalling it if the condition is false. If the condition returns `false`, the plugin is simply not loaded (or not included) for the current session. Useful for environment‑specific configurations (e.g., disabling in VSCode).
**Example**:

```lua
{
  "some/plugin.nvim",
  cond = function()
    return vim.fn.has("macunix") == 1
  end
}
```

---

### `module`

**Meaning**: A boolean flag (or string) that instructs lazy.nvim whether to automatically hook into Lua `require()` to lazy‑load the plugin by module name. If `module = false`, then requiring the module will *not* automatically trigger loading. Useful for subtle control.
**Example**:

```lua
{
  "nvim-tree/nvim-web-devicons",
  lazy = true,
  module = false
}
```

---

### `dir` / `url` / `dev`

**Meaning**: Fields for more advanced scenarios:

* `dir`: path to a local plugin directory (for when you develop a plugin locally).
* `url`: custom repository URL (instead of the default GitHub `owner/repo`).
* `dev`: boolean; if `true`, treat the plugin as a local dev version (skipping remote fetch).
  **Example**:

```lua
{ dir = "~/projects/my‑plugin.nvim" }
{ url = "git@github.com:myuser/custom.nvim.git", dev = true }
```

---
