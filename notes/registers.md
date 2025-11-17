# Registers

## Definition and Purpose

In Neovim, a *register* is a named storage location within the editor reserved for text, commands, or actions. When you perform operations such as yank (copy), delete (cut), change, or record macros, the content is stored in one or more registers for later retrieval. Registers are accessed by prefixing the register name with a double-quote (`"`), followed by the register identifier and then the operation (for example, `"ap`).

The use of registers enables more granular control than a single clipboard: you may store multiple distinct pieces of data, choose which one to paste or apply, and avoid overwriting useful contents inadvertently.

---

## Register Categories

Neovim supports multiple categories of registers. The following breakdown summarises the major types, along with their typical uses.

### Unnamed Register (`""`)

This is the default register used implicitly when no explicit register is specified. For example, when you type `yy` (yank a line) without specifying a register, the line is stored in the unnamed register. When you then press `p` to paste, `p` uses the unnamed register unless another is specified.

### Numbered Registers (`"0` to `"9`)

These registers are automatically populated by the system:

* Register `"0` holds the most recent yank (when you yank entire lines) and is not overwritten by deletes.
* Registers `"1` through `"9` hold the most recent deletes (and changes) in decreasing order of recency. For example, `"1` has the most recent delete, `"2` the previous one, and so on.

These numbered registers are useful when you want to retrieve earlier deletions or yanks that would otherwise be overwritten in the unnamed register.

### Named Registers (`"a` to `"z` and `"A` to `"Z`)

These are user-designated registers named by lowercase letters. You explicitly select them by prefixing commands. For example, `"ayy` yanks a line into register `a`. To append to a named register rather than overwrite it, you use uppercase: for example `"Ayy` appends the yanked line to register `a`. This gives you control to store text under meaningful labels for later retrieval.

### Special Registers

These include registers with fixed special purposes:

* Clipboard/selection registers: `"*`, `"+`; used for system clipboard or selection buffers.
* Black hole register: `"_`; prevents text from being saved (effectively discards).
* Expression register: `"=`; allows you to evaluate an expression and store its result.
* Read-only registers: for example `"."` (last inserted text), `": "` (last command), `"%"` (current file name).
* Alternate file register: `"#`; contains the name of the alternate file (previously edited file).

### Small-delete Register (`"-`)

The `"-` register is used for small deletes (i.e., deletions less than a full line) so that such deletes do not overwrite numbered registers.

---

## Basic Usage Patterns

### Storing (Yank/Delete)

To store text into a register explicitly:

* Use `"x` where `x` is the register name, followed by the command. Example: `"ayy` (yank a line into register a)
* To delete into a register: `"bdw` would delete a word into register `b` (replace `dw` with your delete operator).
  If no register is specified, the unnamed register `""` is used.

### Retrieving / Pasting

* In Normal mode: `"ap` will paste the content of register `a` after the cursor.
* `"aP` will paste before the cursor.
* In Insert mode: use `Ctrl-R` followed by the register name, e.g., `Ctrl-R a` to insert contents of register `a` at cursor position.
* On the command line (Ex mode) you can also use `:put {register}` to insert below the current line. For example `:put a` will put the contents of register `a` on a new line.

### Viewing Registers

You can inspect the contents of registers using the command `:reg` (or `:registers`).
You may specify one or more register names to filter. Example: `:reg a b c` will show the contents of registers `a`, `b`, and `c`.

### Macros and Recording into Registers

Registers are also used to record macros:

* Start recording: `q{register}` (e.g., `qa`) to record into register `a`.
* Perform the sequence of operations you wish to record.
* Stop recording: press `q`.
* Execute the macro: `@{register}` (e.g., `@a`).
* To repeat the most recent macro: `@@`.
* You may supply a count before the execution: e.g., `10@a` executes register `a` ten times.

---

## Advanced Considerations & Best Practices

### Choosing which register to use

* Use the unnamed register when you simply want default behavior.
* Use named registers when you have multiple pieces of text you want to reuse independently.
* Use system clipboard registers (`"+`, `"*`) to share text between Neovim and external applications.
* Use the black‐hole register `"_` when you wish to discard deletions without polluting the registers.
* Use numbered registers when you may want to access older deletes or yanks beyond the most recent.

### Avoiding accidental overwrites

Since many operations implicitly overwrite registers (especially the unnamed and numbered registers), if you have text you wish to preserve, explicitly yank it into a named register early.
Also avoid using a register for a macro or text storage if you might need its current contents.

### Combining with macros

Since macros are stored in registers, you can build complex workflows: record a macro into register `a`, then execute it many times via `@a` or `10@a`. You may also yank a macro into a named register for later reuse.
One caveat: if you record a macro into a register that currently holds text you need, you will overwrite that text.

### Clipboard integration

If you have configured Neovim to use the system clipboard (typically via the `clipboard` option including `"unnamed"` or `"unnamedplus"`), then yank/delete operations may also populate the system clipboard register (`"+`) or selection register (`"*`).
To explicitly yank into the system clipboard: `"+y{motion}` or `"+yy`.
To explicitly paste from the system clipboard: `"+p` or `"+P`.

### Expression register and dynamic content

The expression register `"=` allows evaluating an expression in insert mode. For example, in Insert mode you can press `Ctrl-R =`, type `2+2<Enter>`, and then “4” is inserted. You can use this to insert computed or queried data directly into your buffer via register mechanism.

### Register contents and memory

Note that registers persist only for the duration of the session (unless using shada or equivalent mechanisms). You may also clear or overwrite registers intentionally; for example via `:let @a = ''` will clear register `a`.

---

## Example Scenarios

### Example A: Named register usage

1. You want to yank a function signature into register `f`:

   ```
   "fy}      -- yank a paragraph into register f  
   ```
2. Later, move to where you need it and paste:

   ```
   "fp       -- paste contents of register f  
   ```

### Example B: Macros in registers

1. Record macro into register `m`:

   ```
   qm          -- start recording into register m  
   ^           -- go to the first non-blank character of the line  
   iTODO: <Esc>- insert “TODO: ”  
   j           -- go to next line  
   q           -- stop recording  
   ```
2. Execute macro 5 times:

   ```
   5@m  
   ```
3. Use `@@` to repeat the macro just executed.

### Example C: Clipboard register

On a system with clipboard support:

```
"+yy      -- yank the current line into system clipboard  
"+p       -- paste from system clipboard  
```

### Example D: Expression register

In Insert mode:

```
Ctrl-R =2+2<Enter>  
```

This results in “4” inserted at cursor position.

---
