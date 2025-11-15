### `j`  

**Function:**  
Move the cursor one line down. *Exclusive* of any selection by itself.  

**Syntax:**  
`j`, `2j`, `10j`.  

**Description:**  
The key `j` advances the cursor downwards in the file by line. It respects the screen layout (wrapped lines vs physical lines) depending on settings like `'wrap'`. With a count, it moves multiple lines. Useful for scanning vertically.  

**Examples:**  
1. *Basic:* `j` moves down one line.  
2. *Intermediate:* `5j` moves down five lines.  
3. *Advanced:* `dj` deletes the current line and the next line (operator‑motion).  

**Common Pairings:**  
`dj`, `yj`, `cj`, `3j`.  

**Notes:**  
When moving down across folded lines or across screen boundaries, actual motion may be affected by folds or soft‑wrapping.  

---
