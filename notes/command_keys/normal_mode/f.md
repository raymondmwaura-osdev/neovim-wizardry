### `f`  

**Function:**  
Locate and move the cursor to the *next* occurrence of a specified character on the current line.  
This motion is *inclusive*; the cursor rests directly on the matched character.

**Syntax:**  
`f<char>`  
Operator forms: `df<char>`, `yf<char>`, `cf<char>`  
Count forms: `3f<char>`

**Description:**  
The `f` motion executes a forward, intra-line search for a single ASCII character.  
If a count is supplied, Vim resolves the target as the *n*-th subsequent occurrence of that character.  
The search terminates at the line boundary; it does not wrap.  
Failure to find a match leaves the cursor unchanged and terminates the operator (if any) with no effect.

**Examples:**  
1. *Basic:* `f,` moves the cursor to the next comma on the line.  
2. *Intermediate:* `3f(` moves to the third `(` to the right of the cursor.  
3. *Advanced:* `df:` deletes everything from the cursor up to and including the next colon.

**Common Pairings:**  
`d`, `y`, `c`, `gU`, `gu`, counts (`2`, `3`, …)

**Notes:**  
`f` is directionally symmetrical with `F`, which performs the same action in reverse.  
Motion precision depends on literal character matching rather than text-object semantics.

---
