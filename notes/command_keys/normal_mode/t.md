### `t`  

**Function:**  
Move the cursor forward to the position *just before* the next occurrence of a specified character.  
This motion is *exclusive*; the target character itself is not included.

**Syntax:**  
`t<char>`  
Operator forms: `dt<char>`, `yt<char>`, `ct<char>`  
Count forms: `4t<char>`

**Description:**  
`t` performs a forward, partial search for a character, stopping one column before the match.  
Counts index the *n*-th occurrence.  
Like `f`, the search is bounded by the current line.  
Exclusive behavior makes it suitable for precise operator ranges that should not consume the delimiter.

**Examples:**  
1. *Basic:* `t)` moves to the column immediately before the next `)`.  
2. *Intermediate:* `dt:` deletes up to, but not including, the next colon.  
3. *Advanced:* `2ct,` changes text up to the second comma while preserving the delimiter.

**Common Pairings:**  
`d`, `y`, `c`, text transformations such as `gU` and `gu`, counts

**Notes:**  
`t` is the forward counterpart to `T`.  
Its exclusivity is a primary differentiator relative to `f`.

---
