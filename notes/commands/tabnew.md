# `:tabnew`

The `:tabnew` command in Neovim (and Vim) opens a **new tab page**. A tab page is a container for one or more windows; using `:tabnew` you can create a fresh tab (optionally editing a specific file) without disrupting the currently open windows in the existing tab.

---

## Command Syntax

```
:[count]tabnew [++opt] [+cmd] [file]
```

- `count`: an optional tab index; determines where the new tab is opened.
- `file`: optional; the file to open in the new tab.
- `++opt`, `+cmd`: options and commands passed to the editing command, similar to `:edit`.

Count variants:  
- `:tabnew`; create a tab after the current tab.
- `:0tabnew`; create a tab as the **first** tab.
- `:$tabnew`; create a tab after the **last** tab.
- `:-tabnew` (or `:-1tabnew`); open a tab just before the current tab.

---

## Usage Examples

```
:tabnew
```
Opens a new, empty tab page immediately after the current one.

```
:tabnew README.md
```
Creates a new tab and opens the file `README.md` in it (if that file exists).

```
:3tabnew config.lua
```
Opens a new tab (with `config.lua`) **after** tab number 3.

```
:-tabnew notes.txt
```
Creates a new tab just **before** the current tab, editing `notes.txt`.

---

## Related Commands

- `:tabedit` / `:tabe`; synonym for `:tabnew [file]`.
- `:tabclose` / `:tabc`; close the current tab.
- `:tabonly` / `:tabo`; close all other tabs except the current one.
- `:tabnext` / `:tabn` / `gt`; go to the next tab.
- `:tabprevious` / `:tabp` / `gT`; go to the previous tab.
- `:tabs`; list all open tab pages.

---

## Configuration Interactions

- **Autocommands**: You can use `TabEnter` or `TabLeave` autocommand events to run logic when entering or leaving a tab. For instance, to trigger custom behaviour on tab creation:  
  ```lua
  vim.api.nvim_create_autocmd("TabNew", {
    callback = function()
      print("New tab created!")
    end,
  })
  ```

The `:tabnew` command triggers a sequence of autocommand events: `WinLeave`, then `TabLeave`, `WinEnter`, `TabEnter`, `BufLeave`, and `BufEnter` (for the buffer in the newly created tab).
* **Tabline configuration**: The `'tabline'` and `'showtabline'` options in your `init.lua` or `init.vim` control how tab labels are rendered in the UI, which influences how noticeable newly created tabs are.
* **Key mappings**: In a Lua-based config, you might bind a key to open a new tab:

  ```lua
  vim.keymap.set("n", "<C-t>", "<cmd>tabnew<cr>", { desc = "Open new tab" })
  ```

  This mapping is commonly used to streamline creating new tab pages.

---
