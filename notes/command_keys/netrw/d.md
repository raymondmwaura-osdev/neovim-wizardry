## `d`  

### Function  

In netrw (the built‑in file explorer in Vim/Neovim), pressing `d` prompts you to create a **new directory** in the currently browsed directory.  

### Syntax  

Simply:  
```
d
```
After pressing `d`, you will be prompted to type the name of the directory, then `Enter`.  

### Description  

- The `d` key is mapped in netrw’s *normal mode*.  
- When invoked, netrw asks for the name of the new directory.  
- It then executes a shell command (for local files) or a configured remote command to make the directory. The exact command used depends on the variables `g:netrw_localmkdir` (for local) or `g:netrw_mkdir_cmd` (for remote).
- If you submit an empty name (just press `<CR>` when prompted), the action is **aborted**.
- If the directory already exists (either as a file or as a directory), netrw detects that and *ignores* the creation, showing a message accordingly.
- After creation, netrw may or may not refresh the listing automatically; if it doesn't, you can manually refresh (e.g., using `<C-l>`).  

### Examples  

1. **Basic:** In netrw, press `d`, type `my_folder`, then `Enter`. A directory named `my_folder` is created in the current netrw directory.  
2. **Intermediate:** Suppose you're browsing in `/home/user/docs/`, press `d`, type `2025_reports`, `Enter` → `/home/user/docs/2025_reports/` is created.  
3. **Advanced / Remote:** If you're browsing a remote directory (e.g., over `scp://` or `ftp://`), pressing `d`, naming a directory, and confirming will run a remote `mkdir` command as defined by `g:netrw_mkdir_cmd`.

### Common Pairings  

- `d`; create directory  
- `D`; delete file or directory under cursor
- `<CR>`; navigate into a directory  
- `<C-l>`; refresh netrw buffer

### Notes  

- If you get an error like `(netrw) unable to make directory` but the folder still gets created, it may be because your shell setting in Vim/Neovim is wrong. For instance, setting `:set shell=/bin/zsh` fixed this for some users.
- For remote file systems, netrw uses `g:netrw_mkdir_cmd` to determine how to make directories.
- After creating a directory, you may need to manually refresh the listing so that netrw shows the new directory. You can press `<C-l>` or re-enter the directory.

---  
