### `k`  

**Function:**  
Move the cursor one line up. *Exclusive* of selection.  

**Syntax:**  
`k`, `3k`, `15k`.  

**Description:**  
`k` moves the cursor upward by physical lines (not wrapped sub-lines) unless settings alter behavior. A count prefix moves several lines upward. Used for reverse vertical navigation.  

**Examples:**  
1. *Basic:* `k` moves cursor one line up.  
2. *Intermediate:* `7k` moves up seven lines.  
3. *Advanced:* `yk` yanks from current cursor position up one line.

**Common Pairings:**  
`yk`, `dk`, `ck`, `8k`.  

**Notes:**  
When at the first line of buffer, further `k` commands have no effect.  

---
