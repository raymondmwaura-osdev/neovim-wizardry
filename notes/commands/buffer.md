# `:buffer` (alias `:b`)

A command that switches the current window to display a specified buffer, identified by buffer number or name.

---

## Command Syntax

```
:buffer [buffer]
```

- `buffer` can be a buffer number, a full or partial buffer name, or special symbols (e.g., `#` for the alternate buffer).
- `:b` is shorthand for `:buffer`.

---

## Usage Examples

```
:buffer 2
```
Switches to the buffer with number 2.

```
:buffer main.txt
```
Switches to the buffer whose name matches `main.txt`.

```
:buffer uti
```
Switches to the first buffer whose name contains the fragment `uti` (e.g., `utils.py`).

```
:buffer #
```
Switches to the alternate buffer (the buffer you were previously in).

---

## Related Commands

- `:ls` / `:buffers`; list all (or listed) buffers.
- `:bnext` (`:bn`); go to the next buffer.
- `:bprevious` (`:bp`); go to the previous buffer.
- `:bfirst` (`:bf`); go to the first buffer.
- `:blast` (`:bl`); go to the last buffer.
- `:bdelete` (`:bd`); unload and remove a buffer from the buffer list.

---

## Configuration Interactions

- **`buflisted` option**: Only buffers with `buflisted = true` appear in listings like `:ls`, and these are the ones you’ll typically use `:buffer` to switch to.
- **Hidden buffers (`'hidden'` option)**: If `'hidden'` is set, buffers can be switched away without saving, and remain loaded in memory; `:buffer` can still target them.
- **Alternate buffer mechanism**: The `#` register holds the alternate buffer; using `:buffer #` leverages this.

---  
