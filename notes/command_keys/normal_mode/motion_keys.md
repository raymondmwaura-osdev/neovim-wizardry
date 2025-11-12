# Normal Mode Motion Keys  

## Overview  

This document presents a selection of motion keys available in Normal mode in the editor Vim. Motion keys enable the user to navigate the text buffer efficiently without entering Insert or Visual modes.  
These keys often interact with operator commands (such as delete, yank, change) to form compound commands of the form `operator + motion` (for example, `dw` deletes word) and may also interact with counts, registers, marks, and text‑objects. For comprehensive coverage of motions see `:help motion.txt`.  

---

### `h`  

**Function:**  
Move the cursor one character to the left. This motion is *exclusive* in the sense that it moves the cursor position without selecting text.

**Syntax:**  
`h`, `3h`, `5h` (for moving multiple characters).

**Description:**  
The key `h` is one of the foundational horizontal cursor‑movement motions: it shifts the cursor left by one column (or multiple if preceded by a count). It does not wrap lines unless `'whichwrap'` is configured. It is typically used for fine‑grained leftward navigation.

**Examples:**  
1. *Basic:* `h` moves the cursor one character to the left.  
2. *Intermediate:* `4h` moves the cursor four characters to the left.  
3. *Advanced:* `dh` deletes the character to the left of the cursor (via operator‑motion).  

**Common Pairings:**  
`dh`, `yh`, `ch`, `5h`, `2dh`.  

**Notes:**  
If the cursor is at the beginning of a line and `'whichwrap'` does not allow left‑wrapping, `h` will not move further left into previous line.  

---

### `j`  

**Function:**  
Move the cursor one line down. *Exclusive* of any selection by itself.  

**Syntax:**  
`j`, `2j`, `10j`.  

**Description:**  
The key `j` advances the cursor downwards in the file by line. It respects the screen layout (wrapped lines vs physical lines) depending on settings like `'wrap'`. With a count, it moves multiple lines. Useful for scanning vertically.  

**Examples:**  
1. *Basic:* `j` moves down one line.  
2. *Intermediate:* `5j` moves down five lines.  
3. *Advanced:* `dj` deletes the current line and the next line (operator‑motion).  

**Common Pairings:**  
`dj`, `yj`, `cj`, `3j`.  

**Notes:**  
When moving down across folded lines or across screen boundaries, actual motion may be affected by folds or soft‑wrapping.  

---

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

### `w`  

**Function:**  
Move the cursor to the beginning of the next word. The motion is *exclusive* of the current position’s character (i.e., move to the next word start).  

**Mode:**  
Normal mode.  

**Syntax:**  
`w`, `2w`, `5w`.  

**Description:**  
`w` advances the cursor to the first character of the following word. “Word” is defined by Vim’s `iskeyword` and the punctuation/blank segmentation rules. It is used for logical jumps rather than single‑character moves.  

**Examples:**  
1. *Basic:* `w` jumps to the start of the next word.  
2. *Intermediate:* `3w` jumps three word‑starts ahead.  
3. *Advanced:* `dw` deletes from cursor to start of next word via operator‑motion.  

**Common Pairings:**  
`3dw`, `yw`, `cw`, `3w`.  

**Notes:**  
Upper‑case `W` variant interprets “WORD” (delimited by whitespace only) rather than punctuation. Motion with `w` respects punctuation boundaries.

---

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

### `0` (zero)  

**Function:**  
Move the cursor to the first character of the current line (column 1). *Exclusive* of selection.  

**Syntax:**  
`0`

**Description:**  
The key `0` moves the cursor to the literal first column (column 1) of the line, including any whitespace at start. It is a fast way to reset horizontal position on a line.  

**Examples:**  
1. *Basic:* `0` moves cursor to first column of line.  
2. *Intermediate:* `10 0` moves to first column of line 10? (Less common).  
3. *Advanced:* `d0` deletes from current cursor position back to beginning of line (via operator‑motion).  

**Common Pairings:**  
`d0`, `y0`, `c0`, `0`.  

**Notes:**  
Different from `^`, which moves to first non‑blank character.  

---

### `^`  

**Function:**  
Move the cursor to the first non‑blank character of the current line. *Exclusive* of selection.  

**Syntax:**  
`^`

**Description:**  
`^` positions the cursor at the first character of the line that is not whitespace (tabs or spaces). It is useful for quickly aligning to first content on the line rather than column one.  

**Examples:**  
1. *Basic:* `^` moves cursor to first non‑blank of current line.  
2. *Intermediate:* `d^` deletes from current cursor to first non‑blank.  
3. *Advanced:* `y2^` yanks two lines worth of non‑blank leading characters? (Less typical).  

**Common Pairings:**  
`d^`, `y^`, `c^`.  

**Notes:**  
If the line has no indentation (starts at column one), `^` behaves identically to `0`.  

---

### `$`  

**Function:**  
Move the cursor to the end of the current line (last character). *Inclusive* of that character.  

**Syntax:**  
`$`, `5$` (counts apply but seldom used).  

**Description:**  
`$` transports the cursor to the last column of the current line (including trailing whitespace, unless trimmed). It is a standard motion for line termination navigation.  

**Examples:**  
1. *Basic:* `$` moves to end of line.  
2. *Intermediate:* `d$` deletes from cursor to end of line.  
3. *Advanced:* `y$` yanks from cursor to end of line.  

**Common Pairings:**  
`d$`, `y$`, `c$`.  

**Notes:**  
A variant `g_` moves to last non‑blank character of the line.

---

### `gg`  

**Function:**  
Move the cursor to the first line of the buffer (line 1). *Exclusive* of selection.  

**Syntax:**  
`gg`, `10gg` (jump to line 10).  

**Description:**  
`gg` navigates to the beginning of the file (top of buffer). If preceded by a count (for example `10gg`), it moves to that line number. It is a fast way to reposition to the top.  

**Examples:**  
1. *Basic:* `gg` moves to line 1.  
2. *Intermediate:* `15gg` moves to line 15.  
3. *Advanced:* `dgg` deletes from current line to top of file (via operator‑motion).  

**Common Pairings:**  
`dgg`, `ygg`, `cgg`.  

**Notes:**  
`1G` is equivalent to `gg`.  

---

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
