### `T`  

**Function:**  
Move the cursor to the position *just before* the previous occurrence of a specified character.  
This motion is *exclusive*; the character itself is not included.

**Syntax:**  
`T<char>`  
Operator forms: `dT<char>`, `yT<char>`, `cT<char>`  
Count forms: `3T<char>`

**Description:**  
`T` performs a reverse, intra-line search, stopping one column before the matched character.  
Counts select the *n*-th prior occurrence.  
The search halts at the beginning of the line and does not wrap.  
Exclusive movement supports delimiter-sensitive editing operations.

**Examples:**  
1. *Basic:* `T=` stops one character before the preceding `=`.  
2. *Intermediate:* `dT(` deletes backward up to, but not including, the preceding `(`.  
3. *Advanced:* `3cT,` changes text back to just before the third prior comma.

**Common Pairings:**  
`d`, `y`, `c`, case-modification operators, counts

**Notes:**  
`T` complements `t` for reverse directional editing.  
Operational semantics match those of `t`, differing only in direction.

---
