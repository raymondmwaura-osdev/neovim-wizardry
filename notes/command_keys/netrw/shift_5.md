## `%`  

### Function

In NetRW (Vim/Neovim’s built‑in directory browser), pressing `%` opens a prompt to **create a new file** in the *current NetRW directory* (i.e., `b:netrw_curdir`).
The new file is created *if it does not already exist*; if it exists, it is opened.

### Syntax

```
%
```

(The key is typically pressed in normal mode when focused in a NetRW buffer.)

### Description

The `%` key in NetRW triggers a NetRW‑specific mapping that requests a filename from the user. After the filename is entered, NetRW will create an *empty file* at that path (within its current directory) and then open it in a buffer.
Because the action targets `b:netrw_curdir`, it is sensitive to how NetRW’s current directory is defined: depending on your configuration or how you opened NetRW, the actual filesystem path where the file is created may differ from the directory visually under the cursor in the NetRW listing.
By default, after creation, the new file opens in the same window (NetRW buffer) unless this behavior is remapped.

### Examples

1. **Basic:**  
   In NetRW, press `%`, type `newfile.txt`, then press Enter. A new file `newfile.txt` is created in NetRW’s current directory and opened in a buffer.  

2. **Intermediate:**  
   Suppose your NetRW buffer shows `/home/user/project/`. You press `%`, then enter `notes/todo.md`. NetRW will create `todo.md` inside a subfolder `notes` (relative to `project`) and open it, assuming the path is valid relative to `b:netrw_curdir`.  

3. **Advanced / contextual:**  
   If you're using a split layout via `:Lexplore` (NetRW in a split), pressing `%` may create the file in the *Vim working directory* rather than the NetRW-visible directory, depending on `g:netrw_keepdir` and how `g:netrw_chgwin` is configured.
   - For example, users sometimes remap `%` so that new files open in a different (preview) window or split:  
     ```vim
     autocmd filetype netrw call Netrw_mappings()
     function! Netrw_mappings()
       noremap <buffer>% :call CreateInLastWindow()<cr>
     endfunction
     function! CreateInLastWindow()
       let l:filename = input("new file's name: ")
       let l:netrwdir = b:netrw_curdir
       execute g:netrw_chgwin . "wincmd w"
       execute 'edit ' . l:netrwdir.'/'.l:filename
     endfunction
     ```  
     :contentReference[oaicite:6]{index=6}  

### Common Pairings

- `%` is its own command in NetRW (not combined with operators like `d`/`y` etc.).  
- Often used with directory creation: alongside `d` (for “new directory”) in NetRW.
- When remapped, it may be combined with other commands or functions (see examples above) to control which window the new file opens in.

### Notes

- The `%` mapping is defined in NetRW’s documentation under `:help netrw-%`.
- Because `%` uses `b:netrw_curdir`, the *actual file path* depends on how NetRW’s current directory is set. For instance, if you launched NetRW via `:Lexplore`, the working directory used by `%` may differ from the directory under the cursor.
- If you don’t want the file to open in the NetRW window, you can remap `%`.
- If you just want to create a file without editing it, some users run external shell commands (e.g., `:!touch`) or use custom functions instead of `%`.
- There are known caveats: because of `g:netrw_keepdir`, NetRW may not always stay in sync with your Vim working directory, which affects where `%` will actually create the file.

---  
