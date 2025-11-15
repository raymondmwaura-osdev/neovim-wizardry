### `gg`  

**Function:**  
Move the cursor to the first line of the buffer (line 1). *Exclusive* of selection.  

**Syntax:**  
`gg`, `10gg` (jump to line 10).  

**Description:**  
`gg` navigates to the beginning of the file (top of buffer). If preceded by a count (for example `10gg`), it moves to that line number. It is a fast way to reposition to the top.  

**Examples:**  
1. *Basic:* `gg` moves to line 1.  
2. *Intermediate:* `15gg` moves to line 15.  
3. *Advanced:* `dgg` deletes from current line to top of file (via operator‑motion).  

**Common Pairings:**  
`dgg`, `ygg`, `cgg`.  

**Notes:**  
`1G` is equivalent to `gg`.  

---
