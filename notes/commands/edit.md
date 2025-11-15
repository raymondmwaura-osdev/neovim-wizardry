## `:edit` (alias `:e`)

### Functional Description

`:edit` is a core Neovim command used to load (or reload) a file into the current buffer. It allows you to open a file for editing, switch to another buffer, or discard unsaved changes when reloading.  

### Command Syntax

```
:edit [++opt] [+cmd] [file]
```

Abbreviations:

- `:e`; shorthand for `:edit`  
- `++opt` allows setting file‐reading options (e.g., `++bad`)  
- `+cmd` executes an Ex command after editing  
- `file` is the path to the file to open  

### Behavioral Semantics

- **Interpretation**: When you run `:edit`, Neovim reads the specified file (or re-reads the current buffer’s file if none is given) into a buffer in memory.
- **Preconditions**: 
  - If the current buffer has unsaved changes, `:edit` will fail unless you force it (`:edit!`), or unless settings like `'autowriteall'` are enabled, or if the buffer is hidden (`'hidden'` option).
  - The file must exist (unless you intend to create a new buffer name).  
- **Postconditions / Resulting State**:  
  - The buffer’s file name (its “current file”) is set to the target file.
  - The buffer’s contents reflect the file on disk (or an initial empty buffer if file did not exist).  
  - If `+cmd` is specified, that Ex command is executed immediately after loading.
  - If `++opt` flags (e.g., `++bad=`) are used, file reading may apply special conversion rules during load.
- **Side Effects**:
  - Changes to the alternate file name: When you `:edit` a new file, the previous buffer (if any) may become the alternate file.
  - Modifies the buffer list: the edited file is added if not already present.
  - File‐format detection: when reading a file, Neovim detects end‑of‑line style (`'fileformat'`) and file encoding.
  - Timestamp tracking: Neovim tracks file modification times so that it can warn or reload if the file changes outside Neovim.

### Usage Examples

```
:edit README.md
```
Open the file `README.md` in the current window’s buffer.

```
:e
```
Reload the current buffer’s file from disk.

```
:edit! config.lua
```
Force-reload `config.lua`, discarding any unsaved changes in the current buffer.

```
:edit +/function init config.lua
```
Open `config.lua` and immediately jump to the first line matching the regex `function init` (using the `+cmd` feature).

```
:edit ++bad=drop binaryfile.dat
```
Open `binaryfile.dat`, dropping illegal or unconvertible bytes during load.

---

## Related Commands

- `:write` (`:w`); save buffer contents to file  
- `:quit` (`:q`); close the buffer / window  
- `:next` / `:prev`; navigate through argument list of files
- `:buffer`; switch to or load a buffer by buffer number or name  
- `:confirm edit`; prompt for confirmation before discarding unsaved changes
- `CTRL-^` (or `CTRL-6`); toggle to alternate file (equivalent to `:e #`)
- `:checktime`; detect external changes and reload buffer if needed

---

## Configuration Interactions

- **`'autowriteall'`**: When enabled, Neovim will try to auto-save the buffer before `:edit` if there are unsaved changes.
- **`'hidden'`**: If this is set, unsaved buffers can be abandoned without forcing; you can `:edit` another file without being forced to save.
- **`'confirm'`**: If enabled, using `:confirm edit` will prompt the user to save or discard unsaved changes.
- **`'fileformats'`**: Determines which end-of-line formats Neovim will try when reading the file.
- **`'fileencoding'`**: When reading a file, encoding detection may be influenced by this; also affects how `++bad` is handled.

---
