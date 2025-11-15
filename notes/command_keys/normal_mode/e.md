### `e`  

**Function:**  
Move the cursor to the end of the current/next word. The motion is *inclusive* of the end of word character.  

**Syntax:**  
`e`, `2e`, `5e`.  

**Description:**  
`e` advances the cursor to the last character of the current word (if already on one) or the next word. It is suitable when one needs to position at word‑end rather than start.  

**Examples:**  
1. *Basic:* `e` moves to the end of the current or next word.  
2. *Intermediate:* `3e` moves to the end of the third word ahead.  
3. *Advanced:* `ce` changes from current cursor to end of next word (delete then insert).  

**Common Pairings:**  
`ce`, `ye`, `de`, `2e`.  

**Notes:**  
Uppercase `E` moves to the end of the next “WORD” (whitespace delimited).

---
