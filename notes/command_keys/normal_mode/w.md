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
