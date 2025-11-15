### `F`  

**Function:**  
Locate and move the cursor to the *previous* occurrence of a specified character on the current line.  
This motion is *inclusive*; the cursor rests directly on the matched character.

**Syntax:**  
`F<char>`  
Operator forms: `dF<char>`, `yF<char>`, `cF<char>`  
Count forms: `2F<char>`

**Description:**  
`F` implements a backward, intra-line search for a single character.  
Counts target the *n*-th preceding occurrence.  
The search is confined to the current line and does not wrap.  
If no match exists, the cursor remains unchanged and any operator is aborted.

**Examples:**  
1. *Basic:* `F{` moves to the nearest `{` to the left.  
2. *Intermediate:* `2F"` moves to the second prior quotation mark.  
3. *Advanced:* `cF=` changes all text from the cursor back through the preceding `=`.

**Common Pairings:**  
`d`, `y`, `c`, `gU`, `gu`, counts

**Notes:**  
`F` mirrors `f` but operates in reverse.  
Exact character fidelity is required; multibyte characters behave according to buffer encoding.

---
