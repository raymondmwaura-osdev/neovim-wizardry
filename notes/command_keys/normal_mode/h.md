### `h`

**Function:**  
Move the cursor one character to the left. This motion is *exclusive* in the sense that it moves the cursor position without selecting text.

**Syntax:**  
`h`, `3h`, `5h` (for moving multiple characters).

**Description:**  
The key `h` is one of the foundational horizontal cursor‑movement motions: it shifts the cursor left by one column (or multiple if preceded by a count). It does not wrap lines unless `'whichwrap'` is configured. It is typically used for fine‑grained leftward navigation.

**Examples:**  
1. *Basic:* `h` moves the cursor one character to the left.  
2. *Intermediate:* `4h` moves the cursor four characters to the left.  
3. *Advanced:* `dh` deletes the character to the left of the cursor (via operator‑motion).  

**Common Pairings:**  
`dh`, `yh`, `ch`, `5h`, `2dh`.  

**Notes:**  
If the cursor is at the beginning of a line and `'whichwrap'` does not allow left‑wrapping, `h` will not move further left into previous line.  

---
