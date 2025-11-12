# Scrolling Commands  

## Overview

This document presents commands in Normal mode that allow the user to scroll the **viewport** independently of the cursor position.  
Unlike typical motion commands, these scrolling keys move the visible portion of the buffer (the window) without repositioning the cursor, allowing the user to read or review text while keeping the editing point fixed.  
These commands can be combined with counts for accelerated scrolling, and they do not interact with operators or registers directly, although operator motions may be used after repositioning the viewport.

---

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

### `Ctrl-y`  

**Function:**  
Scroll the window **up by one line**, moving text downward. The cursor remains on its current line.  
*Exclusive* of any selection or operator effect.  

**Syntax:**  
`Ctrl-y`, `3 Ctrl-y` (scroll multiple lines).  

**Description:**  
`Ctrl-y` moves the visible window up by one line per press, exposing preceding content. With a count, the number of lines scrolled increases proportionally. Useful for reviewing prior lines without moving the cursor.  

**Examples:**  
1. *Basic:* Press `Ctrl-y` to scroll up one line.  
2. *Intermediate:* `4 Ctrl-y` scrolls up four lines.  
3. *Advanced:* Use `Ctrl-y` in combination with `zz` to recenter cursor after review.  

**Common Pairings:**  
Often paired with `Ctrl-e` for smooth vertical review.  

**Notes:**  
Reaching the top of the buffer halts scrolling.  

---

### `Ctrl-d`  

**Function:**  
Scroll **down by half a screen**, cursor remains approximately in place.  
*Exclusive* of selection.  

**Syntax:**  
`Ctrl-d`, `2 Ctrl-d` (scroll multiple half-screens).  

**Description:**  
Moves the viewport downward by roughly half the height of the window. The cursor remains in relative position unless at buffer edges. Commonly used to skim through a file.  

**Examples:**  
1. *Basic:* `Ctrl-d` scrolls half a screen down.  
2. *Intermediate:* `2 Ctrl-d` scrolls one full screen.  
3. *Advanced:* After scrolling, use `zz` to recenter cursor.  

**Common Pairings:**  
`Ctrl-u` for upward half-screen movement; can follow `zz` to adjust cursor.  

**Notes:**  
May move cursor slightly if relative position would exceed window bounds.  

---

### `Ctrl-u`  

**Function:**  
Scroll **up by half a screen**, cursor remains approximately in place.  
*Exclusive* of selection.  

**Syntax:**  
`Ctrl-u`, `2 Ctrl-u` (scroll multiple half-screens).  

**Description:**  
Shifts the viewport upward by roughly half a screen. Preceded by a count, the scrolling distance multiplies accordingly. Useful for quickly reviewing preceding content.  

**Examples:**  
1. *Basic:* `Ctrl-u` scrolls half a screen up.  
2. *Intermediate:* `3 Ctrl-u` scrolls 1.5 screens up.  
3. *Advanced:* Combine with `zz` to recenter after scrolling.  

**Common Pairings:**  
`Ctrl-d` for downward half-screen scrolling.  

**Notes:**  
May slightly adjust cursor to keep it within window bounds.  

---

### `Ctrl-f`  

**Function:**  
Scroll **forward one full screen**, cursor remains in approximate relative position.  
*Exclusive* of selection.  

**Syntax:**  
`Ctrl-f`, `2 Ctrl-f` (scroll multiple full screens).  

**Description:**  
Shifts the viewport forward by a full window, useful for rapid navigation through large files. Cursor remains within the viewport’s relative position, unless buffer limits are reached.  

**Examples:**  
1. *Basic:* `Ctrl-f` scrolls down one screen.  
2. *Intermediate:* `3 Ctrl-f` scrolls three screens.  
3. *Advanced:* Combine with `zz` to center cursor after scrolling.  

**Common Pairings:**  
`Ctrl-b` for backward full-screen scrolling.  

**Notes:**  
Reaching the end of buffer stops further movement.  

---

### `Ctrl-b`  

**Function:**  
Scroll **backward one full screen**, cursor remains in approximate relative position.  
*Exclusive* of selection.  

**Syntax:**  
`Ctrl-b`, `2 Ctrl-b` (scroll multiple full screens).  

**Description:**  
Shifts the viewport backward by a full window. Useful for reviewing previously read text. Preceded by a count, it scrolls multiple screens.  

**Examples:**  
1. *Basic:* `Ctrl-b` scrolls one screen up.  
2. *Intermediate:* `2 Ctrl-b` scrolls two screens.  
3. *Advanced:* Combine with `zz` to recenter the cursor.  

**Common Pairings:**  
`Ctrl-f` for forward screen scrolling.  

**Notes:**  
Stops at buffer top; cursor stays relative to viewport unless forced to adjust.  

---

### `zh`  

**Function:**  
Scroll the window **left by one character**, cursor remains fixed.  
*Exclusive* of selection.  

**Syntax:**  
`zh`, `3 zh`.  

**Description:**  
Moves the viewport left, revealing hidden columns to the left. With a count, scrolls multiple characters.  

**Examples:**  
1. *Basic:* `zh` scrolls left by one column.  
2. *Intermediate:* `5 zh` scrolls left five columns.  
3. *Advanced:* Combine with `zl` to navigate horizontally.  

**Common Pairings:**  
`zl` for rightward horizontal scrolling.  

**Notes:**  
Does not affect the cursor’s logical position within the buffer.  

---

### `zl`  

**Function:**  
Scroll the window **right by one character**, cursor remains fixed.  
*Exclusive* of selection.  

**Syntax:**  
`zl`, `3 zl`.  

**Description:**  
Moves the viewport right, revealing hidden content beyond the current window. With a count, scrolls multiple characters.  

**Examples:**  
1. *Basic:* `zl` scrolls right by one column.  
2. *Intermediate:* `6 zl` scrolls six columns right.  
3. *Advanced:* Use with `zh` for precise horizontal viewport navigation.  

**Common Pairings:**  
`zh` for leftward scrolling.  

**Notes:**  
Cursor remains on the same character in the buffer; viewport only changes.  

---

### `zH`  

**Function:**  
Scroll the window **left by half a screen width**, cursor remains fixed.  

**Syntax:**  
`zH`, `2 zH`.  

**Description:**  
Shifts the viewport left by half the visible width, useful for fast horizontal navigation.  

**Examples:**  
1. *Basic:* `zH` scrolls half-screen left.  
2. *Intermediate:* `2 zH` scrolls a full screen left.  
3. *Advanced:* Combine with `zl`/`zh` for fine adjustments.  

**Common Pairings:**  
`zL` for rightward half-screen scrolling.  

**Notes:**  
Cursor stays in same buffer column; cannot scroll past beginning of line.  

---

### `zL`  

**Function:**  
Scroll the window **right by half a screen width**, cursor remains fixed.  

**Syntax:**  
`zL`, `2 zL`.  

**Description:**  
Shifts viewport right by half the visible width. Useful for reviewing hidden text horizontally in wide lines.  

**Examples:**  
1. *Basic:* `zL` scrolls half-screen right.  
2. *Intermediate:* `2 zL` scrolls a full screen right.  
3. *Advanced:* Combine with `zh`/`zl` for fine-tuned viewport navigation.  

**Common Pairings:**  
`zH` for leftward half-screen scrolling.  

**Notes:**  
Does not move the cursor; horizontal scrolling is limited by line length.  

---
