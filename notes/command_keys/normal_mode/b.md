### `b`  

**Function:**  
Move the cursor to the beginning of the previous word. *Exclusive* of the current position’s character.  

**Syntax:**  
`b`, `2b`, `4b`.  

**Description:**  
`b` moves the cursor leftwards, landing on the first character of the previous word segment (according to word‐definition rules). It is effective for reverse word‑wise navigation.  

**Examples:**  
1. *Basic:* `b` moves to the start of the previous word.  
2. *Intermediate:* `2b` moves back two words.  
3. *Advanced:* `db` deletes from the cursor to the start of the previous word.  

**Common Pairings:**  
`db`, `yb`, `cb`, `2b`.  

**Notes:**  
Uppercase `B` treats “WORD” boundaries (whitespace delimited) rather than punctuation splits.

---
