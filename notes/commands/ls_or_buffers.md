# `:ls` (alias `:buffers`)

`:ls` is a Neovim Ex command that lists all **listed buffers** in the current session. It displays each buffer’s number, status flags, name, and the line position of the cursor when it was last active. This gives you an overview of all the open or remembered files (buffers), which you can then switch to or manage.

---

## Command Syntax

```
:ls
```

- The `!` variant, `:ls!`, lists **all buffers**, including unlisted ones (buffers where `buflisted` is false).
- You can also use the equivalent command `:buffers`.

---

## Usage Examples

```
:ls
```

Lists all *listed* buffers. For instance, it might show:

```
1 %a + "main.py"            line 10
2 h  "notes.txt"            line 1
3  # "README.md"            line 30
```

- `1`; buffer number  
- `%`; current buffer  
- `a`; active (loaded & displayed)  
- `+`; modified  
- `h`; loaded but hidden  
- `#`; alternate buffer (the previous one)

```
:ls!
```

Lists **all** buffers, including unlisted ones, giving you a more complete view of every buffer Neovim knows about.

---

## Related Commands

- `:buffer` (`:b`); switch to a buffer by number or name.
- `:bnext` / `:bn`; move to the next buffer.
- `:bprevious` / `:bp`; move to the previous buffer.
- `:bdelete` (`:bd`); delete (unload) a buffer.
- `:bwipeout` (`:bw`); completely wipe out a buffer (remove from list).

---

## Configuration Interactions

- **`'hidden'` option**:  
  - If `hidden` is set, you can abandon a buffer without saving, but it remains “loaded” and listed, so it shows up in `:ls`.
  - If `nohidden` is set, buffers may be unloaded when abandoned, but they still remain in the buffer list.
- **`buflisted` option**:  
  - Buffers with `buflisted = false` do not show up in `:ls`. To see them, you must use `:ls!`.

---
