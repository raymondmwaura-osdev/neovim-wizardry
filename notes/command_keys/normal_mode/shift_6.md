### `^`  

**Function:**  
Move the cursor to the first non‑blank character of the current line. *Exclusive* of selection.  

**Syntax:**  
`^`

**Description:**  
`^` positions the cursor at the first character of the line that is not whitespace (tabs or spaces). It is useful for quickly aligning to first content on the line rather than column one.  

**Examples:**  
1. *Basic:* `^` moves cursor to first non‑blank of current line.  
2. *Intermediate:* `d^` deletes from current cursor to first non‑blank.  
3. *Advanced:* `y2^` yanks two lines worth of non‑blank leading characters? (Less typical).  

**Common Pairings:**  
`d^`, `y^`, `c^`.  

**Notes:**  
If the line has no indentation (starts at column one), `^` behaves identically to `0`.  

---
