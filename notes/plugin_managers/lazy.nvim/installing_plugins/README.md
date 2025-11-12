# Installing Plugins

## Part 1: User Workflow: Installing a Plugin with lazy.nvim

Suppose you want to install a colourscheme plugin (for example, tokyodark.nvim by tiagovla) using lazy.nvim. Here’s the process:

1. In your Neovim config directory (for Linux/macOS: `~/.config/nvim/`; for Windows: something like `%USERPROFILE%\AppData\Local\nvim\`) you have your `init.lua`‑entry and lazy.nvim bootstrap. For example:

   ```lua
   -- init.lua  
   vim.g.mapleader = " "  
   vim.g.maplocalleader = "\\"  
   require("config.lazy")
   ```

   And in `lua/config/lazy.lua`:

   ```lua
   local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
   if not (vim.uv or vim.loop).fs_stat(lazypath) then
     vim.fn.system({
       "git", "clone", "--filter=blob:none",
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

   (Adapt paths to your OS.)

2. Create a directory `lua/plugins/` (as imported in `spec = { { import = "plugins" } } }`).

3. Inside `lua/plugins/`, create a file, say `colorscheme.lua`, with the plugin specification:

   ```lua
   -- lua/plugins/colorscheme.lua  
   return {
     {
       "tiagovla/tokyodark.nvim",
       lazy = false,
       priority = 1000,
       config = function()
         vim.cmd("colorscheme tokyodark")
       end,
     },
   }
   ```

4. Save your files. Start (or restart) Neovim.

5. In Neovim you can open the lazy.nvim UI with `:Lazy` (if configured). Then you can install missing plugins or sync. Example commands: `:Lazy install`, `:Lazy sync`.

6. Once the plugin is installed, you should see the colourscheme applied (because your `config` runs).

7. If you ever change the spec (add or remove plugins) you run the sync/clean operations through the UI or CLI of lazy.nvim.

---

## Part 2: Internal Mechanics: How lazy.nvim Installs and Loads Plugins

Now let’s break down what lazy.nvim is doing behind the scenes when you add that spec and start Neovim. This explains its internal mechanics and rationale.

### Loading your configuration and collecting plugin specs

* When you call `require("lazy").setup({ spec = { { import = "plugins" } } })`, lazy.nvim reads the `spec` parameter, which points to modules (in this case all Lua files under `lua/plugins/`).
* It then **loads** those modules (executing the `return { … }` tables) and constructs an internal list of “plugin specs” (metadata about each plugin) based on the tables you returned.
* Each spec describes things like: the plugin repository identifier (e.g., `"tiagovla/tokyodark.nvim"`), whether it should be lazy‑loaded, what events trigger it, dependencies, build commands, config/init functions, etc.
* Lazy.nvim also resolves dependencies between plugins.

### Bootstrapping / Installing plugins

* After building the spec list, lazy.nvim checks for **missing plugins** (those in your spec but not yet installed locally). One of its features is “Automatically install missing plugins before starting up Neovim”.
* For each missing plugin it will clone the repository (usually via Git). It uses partial clones / shallow clones when possible (“--filter=blob:none” etc) to reduce size.
* It stores plugins in a dedicated directory under Neovim’s `stdpath("data")`, commonly `~/.local/share/nvim/lazy/…` (or equivalent) so they are not mixed with other config files.
* After cloning, it may run a “build” command if the spec includes a `build` property (for plugins that require compilation or installation of external tools).

### Runtime‑path adjustment and loading

* Once installed, lazy.nvim adjusts Neovim’s `runtimepath` (rtp) so that the plugin’s paths are included (so Neovim can find the plugin’s `plugin/`, `lua/`, `autoload/`, etc).
* For plugins marked `lazy = false` (i.e., start plugins) lazy.nvim loads them immediately (during startup) so that they are available right away.
* For plugins marked `lazy = true` (or with triggers like `event`, `cmd`, `ft`, `keys`), lazy.nvim *defers* loading until the trigger condition occurs (e.g., user opens a file of a given filetype, or executes a command, or presses a key mapping). This improves startup performance by not loading everything upfront.

### Configuration of the plugin once loaded

* After lazy.nvim loads a plugin, it executes the `init` function (if provided) *before* plugin load.
* Then when the plugin is actually loaded, lazy.nvim executes the `config` function (or auto‑setup using `opts` if you provided it).
* Example: In our colourscheme spec we set `config = function() vim.cmd("colorscheme tokyodark") end`. So when it loads, lazy.nvim ensures that code runs.

### Managing plugin updates, versioning, lockfile

* Lazy.nvim supports versioning via `branch`, `tag`, `commit`, or `version` fields in the spec. This lets you pin a plugin to a specific version.
* It maintains a **lockfile** (often `lazy-lock.json`) to track exactly which versions are installed, so future installs are reproducible.
* It offers commands or UI to update all plugins, check for updates and clean unused plugins. Example: “Update: shift+U, Sync: shift+S, Clean: shift+X”.

### Performance optimisations

* Lazy.nvim performs bytecode compilation / caching of Lua modules to improve startup times.
* It also handles **lazy‑loading** so only necessary plugins load at startup, reducing overhead.
* It ensures correct load order among dependencies so one plugin is loaded after another if required, so you don’t have to manually script load sequencing.

---
