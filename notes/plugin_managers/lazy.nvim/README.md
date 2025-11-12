# Lazy.Nvim

## What is lazy.nvim

Lazy.nvim is a modern plugin manager for Neovim. It handles:

* Installing and updating plugins (from e.g. GitHub).
* Adjusting the runtime path so Neovim loads the plugins.
* Lazy-loading plugins on demand (on events, commands, filetypes, keymaps) to improve startup performance.
* Structuring your plugin definitions declaratively so you keep your config cleaner and more maintainable.

In effect: once configured, you declare your plugins (with metadata: when to load, dependencies, build steps, etc) and lazy.nvim orchestrates installing, loading, updating them.

---

## Prerequisites

* You need Neovim version compatible with lazy.nvim (e.g., v0.10.0 or newer) in many guides.
* Git must be available (for cloning plugin repos).
* A basic Lua-based Neovim config (i.e., you’re comfortable editing your `init.lua` and `lua/` directory).

---

## Installation

1. In `~/.config/nvim/init.lua` (or equivalent on your OS) set up loading lazy.nvim:

   ```lua
   require("config.lazy")
   ```

   (Assuming you’ll place lazy’s setup in `lua/config/lazy.lua`.)
2. In `lua/config/lazy.lua` insert:

   ```lua
   local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
   if not (vim.uv or vim.loop).fs_stat(lazypath) then
     vim.fn.system({
       "git",
       "clone",
       "--filter=blob:none",
       "--branch=stable",
       "https://github.com/folke/lazy.nvim.git",
       lazypath,
     })
   end
   vim.opt.rtp:prepend(lazypath)

   require("lazy").setup({
     spec = {
       { import = "plugins" },
     },
     install = { colorscheme = { "habamax" } },
     checker = { enabled = true },
   })
   ```

   This clones lazy.nvim if it’s not present, prepends to Neovim’s `runtimepath`, then calls its `.setup()` with a specification of where your plugin-spec files live.
3. Create the `lua/plugins/` directory (or whatever directory you referenced in `spec`) and add plugin definition files there.
4. After starting Neovim you may run:

   ```vim
   :checkhealth lazy
   ```

   to verify installation.

---

## Structuring plugin definitions

In `lua/plugins/` you create Lua files returning tables that describe plugins. Example:

```lua
-- lua/plugins/colorscheme.lua
return {
  "tiagovla/tokyodark.nvim",
  lazy = false,
  priority = 1000,
  config = function()
    vim.cmd("colorscheme tokyodark")
  end,
}
```

Key fields you will see:

* The first value in the table: the plugin repo string (e.g. `"author/repo"`).
* `lazy` (boolean) telling if plugin is loaded lazily or upfront.
* `priority` to enforce load order when necessary.
* `event`, `cmd`, `ft`, `keys`: triggers for lazy-loading on filetype, command, key mapping, etc.
* `dependencies`: other plugins that must load first.
* `build`: command to run after installation or update (e.g., `":TSUpdate"`).
* `config` or `opts`: function (or table) to configure the plugin once loaded.

---

## Basic workflow and commands

After config:

* Launch Neovim → lazy.nvim will install any missing plugins referenced in your specs.
* You can open the UI of lazy.nvim with `:Lazy`.
* Within the UI you can perform operations: update all, sync, clean unused. Example shortcuts include: `shift+U` for update, `shift+S` for sync, etc.
* Adding a new plugin: add its spec in the `lua/plugins/` directory, then run `:Lazy sync` (or similar) to install.
* Removing/un-referencing a plugin: remove or comment out its spec and run the clean/cleanup command inside lazy.nvim.

---

## Benefits

* Improved startup performance because you can set plugins to load only when needed (filetype, command, event).
* Better organization: you keep plugin list declarative, in Lua modules, separated by concern (themes, LSP, UI, etc).
* Dependency and build support: ensures that when a plugin says “I depend on X”, lazy.nvim installs X first and performs build steps if required.
* Automatic checking for updates (if enabled) so your plugin list stays current.
* Unified UI for plugin management inside Neovim rather than managing manually via git clones or separate scripts.

---

## Caveats and things to watch

* Because many plugins can load lazily, be careful with configurations that assume the plugin is always loaded. If you write config code outside of the plugin spec and the plugin hasn’t loaded, you may get errors.
* If you lazy-load a plugin based on an event / filetype you must ensure that the trigger is correct. Mis-specified triggers lead to plugins not loading when you expect.
* Some legacy plugins may expect to be loaded at startup; loading them lazily might cause unexpected behaviour.
* While lazy.nvim reduces manual work, you still need to understand your plugin ecosystem (what each plugin does, what its dependencies are); the manager doesn’t automate conceptual choices for you.

---
