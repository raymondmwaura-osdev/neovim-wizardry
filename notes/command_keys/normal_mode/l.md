### `l`  

**Function:**  
Move the cursor one character to the right. *Exclusive* of selection.  

**Syntax:**  
`l`, `2l`, `10l`.  

**Description:**  
The `l` key is the rightward counterpart to `h`. It moves the cursor one position to the right (or multiple if a count is given). It is efficient for character‑level rightward traversal, especially staying on the home row.  

**Examples:**  
1. *Basic:* `l` moves the cursor right one character.  
2. *Intermediate:* `6l` moves the cursor six characters to the right.  
3. *Advanced:* `dl` deletes the character immediately to the right of the cursor.  

**Common Pairings:**  
`dl`, `yl`, `cl`, `4l`.  

**Notes:**  
If the cursor is at end of line and `'whichwrap'` is not set for right‑wrapping, `l` will not traverse to the next line.  

---
