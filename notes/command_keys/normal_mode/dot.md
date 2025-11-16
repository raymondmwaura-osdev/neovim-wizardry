## `.`  
*(dot)*  

### Function

In Neovim (and Vim), the `.` key in Normal mode **repeats the most recent buffer change** (i.e., insertions, deletions, replacements).
It does **not** repeat a command-line (`:`) command.

### Syntax

- `.`  
- `[count].`; with a numeric prefix, you repeat the last change that many times (overriding the original count).

### Description

- When you perform a change in Normal mode (for example, via `d`, `c`, or entering Insert mode), Neovim stores that change as the “last change.”
- Pressing `.` re-executes that exact change, applying it at the current cursor position (or context), with the same internal details (motions, insert content, etc.).
- If you supply a count before `.`, that count **replaces** the count used in the original change (not multiplies it).
- When the last change used a specific numbered register (like `"1p`, etc.), repeating with `.` may increment the register number.
- In the case where the previous change was done using a **Visual** selection, when `.` is used, the **same size** of the region is targeted.

### Examples

1. **Basic**  
   - Go into Insert mode: `iHello<Esc>`; inserts “Hello”.  
   - Press `.` → Neovim will insert “Hello” again at the current cursor position.

2. **Intermediate**  
   - Delete a word: `dw` (delete up to the start of the next word).  
   - Move somewhere else; press `.` → deletes the next word in the same way.  

3. **Advanced / Contextual**  
   - Suppose you have several lines each ending in `;` in a code file.  
   - You go to the end of a line, press `A;<Esc>` to append a semicolon, then move down one line (`j`).  
   - Now you can do `.` → it will append `;` at the end of the new line, and you can repeat `j.` to keep doing that across lines.
   - Another: after using a change command with a motion like `cgnnewtext<Esc>` (changing the last search match), pressing `.` will repeat that change on the next match.

### Common Pairings

- With delete: `d` + motion → then `.` repeats deletion.  
- With change: `c` + motion → then `.` repeats that change.  
- With insertion: `i`, `a`, `o`, `A`, etc. → then `.` repeats insertion.  
- With counts: e.g. `3dw` then `.`  
- With Visual-mode changes: e.g. select some text then `c` or `d` → `.` repeats on same-sized region.

### Notes

- `.` only records **buffer changes**, not other kinds of commands (like Ex `:` commands).
- When combining with a count, the new count **replaces** the old one; it doesn’t multiply.
- For changes that came from Visual mode, the **same region size** is reused, which means if your selection was 5 characters, `.` will target a region of 5 characters even if your cursor moved.
- The dot command is one of Vim/Neovim’s most powerful productivity tools: by performing a well-structured change, you can apply it repeatedly with minimal keystrokes.

---  
