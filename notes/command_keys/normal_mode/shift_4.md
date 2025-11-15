### `$`  

**Function:**  
Move the cursor to the end of the current line (last character). *Inclusive* of that character.  

**Syntax:**  
`$`, `5$` (counts apply but seldom used).  

**Description:**  
`$` transports the cursor to the last column of the current line (including trailing whitespace, unless trimmed). It is a standard motion for line termination navigation.  

**Examples:**  
1. *Basic:* `$` moves to end of line.  
2. *Intermediate:* `d$` deletes from cursor to end of line.  
3. *Advanced:* `y$` yanks from cursor to end of line.  

**Common Pairings:**  
`d$`, `y$`, `c$`.  

**Notes:**  
A variant `g_` moves to last non‑blank character of the line.

---
