### `Ctrl-e`  

**Function:**  
Scroll the window **down by one line**, moving text upward. The cursor remains on its current line.  
*Exclusive* of any selection or operator effect.  

**Syntax:**  
`Ctrl-e`, `3 Ctrl-e` (scroll multiple lines).  

**Description:**  
`Ctrl-e` shifts the viewport down by one line per press, revealing the next line(s) in the buffer while keeping the cursor fixed. When preceded by a count, the viewport moves that many lines. Useful for reading or reviewing text without altering the cursor location.  

**Examples:**  
1. *Basic:* Press `Ctrl-e` to scroll down one line.  
2. *Intermediate:* `5 Ctrl-e` scrolls down five lines.  
3. *Advanced:* Scroll down while using `zz` afterward to center the cursor if needed.  

**Common Pairings:**  
Can be used with `zz` or `zt`/`zb` to adjust viewport alignment.  

**Notes:**  
Does not affect the cursor’s logical position. Reaching the buffer bottom stops further scrolling.  

---
