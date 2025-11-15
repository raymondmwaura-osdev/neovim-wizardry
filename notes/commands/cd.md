# `:cd`

The `:cd` command in Neovim (and Vim) changes the current working directory (cwd) of the editor process. This affects file‑resolution for Ex commands, external commands (e.g., `:!ls`), and relative file paths. The scope of `:cd` is **global** by default, meaning it applies to all windows and tabs unless overridden by more specific directory commands like `:tcd` or `:lcd`.

---

## Command Syntax

```
:cd [!] [path]
```

- Without arguments, `:cd` changes to the home directory (when `cdhome` is enabled).
- With a path, it changes the working directory to that path, possibly searching in the `cdpath` if the path is relative.
- The optional `!` forces a global change and clears any window-local directory.
- `:cd -` changes to the previous working directory.

---

## Usage Examples

```
:cd /home/user/projects
```
This sets the Neovim global working directory to `/home/user/projects`, so subsequent edits or external commands will resolve relative to that directory.

```
:cd ..
```
Moves the working directory one level up (to the parent directory), assuming a Unix‑style filesystem.

```
:cd -
```
Returns to the previous working directory (the directory before the last `:cd` invocation).

```
:cd %:p:h
```
Changes the working directory to the directory containing the current buffer’s file. Here `%` expands to the current file, `:p` makes it absolute, and `:h` removes the filename to yield the containing directory.

---

## Related Commands

- `:pwd`; Prints the current working directory.
- `:tcd`; Changes the working directory for the **current tab only**.
- `:lcd`; Changes the working directory for the **current window only**.
- `:chdir` or `:chd`; Synonyms for `:cd`.
- Option: `'autochdir'`; When set, automatically changes cwd to the directory of the opened buffer.

---

## Configuration Interactions

In your `init.lua` (or `init.vim`), you can influence how `:cd` behaves via:

1. **Autocommands**:  
   For example, to set cwd when Neovim starts or when entering a buffer:

   ```lua
   vim.api.nvim_create_autocmd("VimEnter", {
     callback = function()
       vim.cmd("cd " .. vim.fn.getcwd())
     end,
   })
   ```

Alternatively, to always `cd` into the directory of the current file on buffer enter:

```lua
vim.api.nvim_create_autocmd("BufEnter", {
  callback = function()
    vim.cmd("cd " .. vim.fn.expand("%:p:h"))
  end,
})
```

2. **Option `'autochdir'`**:
   Enabling this option in your config causes Neovim to change its cwd automatically whenever you open a file, switch buffers, or open/close windows.
3. **Plugins that use cwd**:
   Plugins such as workspace managers (e.g. `workspaces.nvim`) allow configuration of which `cd`‑type to use (`"global"`, `"tab"`, `"local"`) when changing directories programmatically.
4. **Clearing local scopes**:
   When you use `:cd`, any window-local (`:lcd`) or tab-local (`:tcd`) directories are cleared, restoring global scoping.

---
