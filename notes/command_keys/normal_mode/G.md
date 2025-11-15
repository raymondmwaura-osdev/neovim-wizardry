### `G`  

**Function:**  
Move the cursor to the last line of the buffer (end of file). *Exclusive* of selection.  

**Syntax:**  
`G`, `20G` (go to line 20).  

**Description:**  
`G` by itself navigates to the final line of the file. When prefixed by a count, it jumps to that line number instead (e.g., `20G` moves to line 20). It enables rapid movement to the file’s end.  

**Examples:**  
1. *Basic:* `G` goes to last line.  
2. *Intermediate:* `30G` jumps to line 30.  
3. *Advanced:* `yG` yanks from current cursor to end of file.  

**Common Pairings:**  
`dG`, `yG`, `cG`.  

**Notes:**  
When used in conjunction with `$`, e.g., `G$`, one reaches the last character of the file’s last line.

---
