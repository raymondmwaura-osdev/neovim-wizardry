# NetRW

* **NetRW** stands for *network read/write*. It is included by default in (Neo)Vim and provides functionality to browse directories, open files, and even operate on remote file systems.
* It supports multiple protocols, including **scp**, **ftp**, **http**, **dav**, and more, allowing you to transparently read/write files on remote machines.
* It is not just a passive browser: you can create, delete, rename, move files and directories via NetRW.

---

## Enabling and Starting NetRW in Neovim

1. **Ensure plugins are enabled**
   To make sure NetRW works, your configuration should enable Vim’s plugin system. In your `init.vim` (or `init.lua`), ensure:

   ```vim
   set nocompatible
   filetype plugin on
   ```

   This ensures NetRW’s scripts and autoload components are active.

2. **Open NetRW**

   * To browse the current working directory, you can run:

     ```
     :e .
     ```
   * Alternatively, you can split the window in different ways:
     * `:Ex`; open in full window
     * `:Vex`; vertical split explorer
     * `:Sex`; horizontal split

---

## Navigating Within NetRW

Once you're in a NetRW buffer:

* Use normal Vim motions (`j`, `k`, etc.) to move the cursor among files and directories.
* To **enter a directory** or **open a file**, press `<Enter>` (the **CR** key).
* To go **up one directory level**, move to the `../` entry and press `<Enter>`.

---

## Viewing Style and Layout

* You can change how listings are displayed by pressing **`i`** (NetRW list-style toggle). There are several styles: *thin*, *long*, *wide*, *tree*.
* To make a particular style permanent, set the variable `g:netrw_liststyle` in your config. For example:

  ```vim
  let g:netrw_liststyle = 3  " tree-style by default
  ```

* You can configure the size of the NetRW window. `g:netrw_winsize` controls initial width/height when NetRW opens.

---

## Opening Files: Splits, Tabs, Previous Window

NetRW supports flexible opening of files:

* **Vertical split**: press `v` on a file/directory → opens it in a vertical split.
* **Horizontal split**: press `o` → new horizontal split.
* **Open in a new tab**: press `t` on a file.
* **Reuse the previous window**: press `P` to open a file in whatever window was used before.

---

## File and Directory Operations

NetRW supports common file operations. Here are some key mappings and commands:

* `%` → create a **new file** in the current directory.
* `d` → create a **new directory**.
* `D` → delete the selected file or directory (directories must be empty).
* `R` → **rename** the selected file or directory.

For moving and copying:

1. Use `mf` to **mark a file**.
2. Use `mt` to **mark a target directory**.
3. Then `mm` to **move** marked files to the target.

---

## Refreshing the Directory Listing

* Press `<C-l>` (Control‑L) in a NetRW buffer to refresh the listing.
* Alternatively, hitting `<Enter>` on the `./` line will re-read the directory.

---

## Bookmarks

NetRW has a bookmarking system:

* `:NetrwMB`; add the current file/directory (or marked ones) to bookmarks.
* `:NetrwMB!`; delete bookmarks.
* `g:netrw_home` controls where the bookmark file `.netrwbook` is stored.
* To list bookmarks, use `:NetrwMB` without arguments or use `:Netrw`-history commands.

---

## Remote File Editing

One of NetRW’s powerful features is **transparent remote editing**:

* You can open a remote file using a URL, e.g.:

  ```
  :e scp://user@hostname/path/to/file
  ```

* For directories, make sure to **include a trailing slash** to prompt NetRW to treat it as a directory and list its contents:

  ```
  :e scp://user@hostname/path/to/directory/
  ```

* After editing, you can save back to the remote location via standard commands (`:w`, `:wq`), and NetRW handles it using the appropriate protocol.

---

## Good Practices and Tips

* If you plan to use a **tree‑explorer plugin** (like `nvim-tree.lua`), you may choose to **disable** NetRW, because sometimes they conflict.
* For remote workflows, **set up SSH key-based authentication** to avoid repeated password prompts when using scp or sftp.
* Use NetRW’s bookmarks (`:NetrwMB`) for directories you visit frequently.
* Use preview mode (`g:netrw_preview = 1`) if you often want to inspect file contents without fully opening them.
* Remember: NetRW is powerful and lightweight; for many users, it's enough and avoids the need for installing third-party file-explorer plugins.

---
