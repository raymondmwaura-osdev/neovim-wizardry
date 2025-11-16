# Syntax Highlighting

## Regex‑Based Highlighting

* For decades, editors like Vim used syntax files (`*.vim`) that declared **regex patterns** for language constructs (keywords, strings, comments, etc.).
* These patterns are grouped into *syntax items* (e.g. `match`, `region`) which define how to locate particular spans of text.
* Syntax groups (e.g., `Keyword`, `String`, `Comment`) are then linked to **highlight groups** that control color, font style, and other visual properties.

### How Regex Highlighting Works

* **Pattern matching**: The system uses regex to scan text. Simple things (like a comment starting with `//`) are matched with `:syn match`. More complex, multi-line constructs (like block comments or string literals) use `:syn region`, which defines start and end patterns.
* **State machine & nesting**: Because code often nests (e.g., a string inside a region, or comments inside code), the syntax engine keeps a **state stack**. Regions can be marked `contained`, `contains`, `keepend` and so on, to model nesting.
* **Synchronization**: To avoid rescanning the entire file each time, Neovim uses *sync points*, regex anchors that act like “anchor markers” from which it can resume parsing reliably. Without sync, re-highlighting after edits would be very expensive.
* **Highlight application**: Once text is matched by a syntax item, it’s assigned to a syntax group, then mapped to highlight attributes (color, bold, italic) via highlight groups.
* **Performance trade‑offs**: While regex is fast and straightforward for many patterns, when language constructs become complex (deep nesting, interpolated strings, context-sensitive syntax), regex-based highlighting can falter or become hard to maintain.

### Architecture and File Locations

* Syntax definitions live in **`syntax/x.vim`** files (or in `after/syntax/…`) inside your `runtimepath`.
* When Vim/Neovim loads a buffer, it looks up the filetype, then loads the appropriate syntax script, applies the regex rules, and builds the highlighting state.
* Because these are text-based scripts, they’re relatively trivial to inspect and edit; you can write your own syntax file or tweak existing ones.

---

## Tree‑sitter

* Tree‑sitter is a parsing library / incremental parser generator. It builds a **concrete syntax tree** (or AST-like structure) for your source code.
* Neovim integrates Tree‑sitter to provide more precise, structure-aware highlighting.
* Instead of regex files, Tree‑sitter relies on **language parsers** (compiled libraries) and **query files** that define how syntax nodes map to highlighting.

### How Tree‑sitter Highlighting Works

* **Language Parsers**: For each supported language, there is a parser library (e.g. a `.so` file) that knows how to parse that language. These are placed under `parser/{language}.*` inside a Neovim `runtimepath`.
* **Building the Syntax Tree**: When you open a buffer, Neovim asks Tree‑sitter to **parse** (or re‑parse) the region; thanks to incremental parsing, only changed parts need to be re-parsed.
* **Queries**: Once the tree is built, Neovim runs **Tree‑sitter queries**, which are defined in `.scm` files (Lisp‑like syntax) under `queries/{language}/highlights.scm` in `runtimepath`.
* **Captures & Highlighting**: Queries specify *captures* (named parts in a pattern (e.g. `@variable`, `@function`)) that correspond to node types in the tree. Those captures are mapped to highlight groups.
* **Priority & Extmarks**: Tree‑sitter uses **extmarks** (Neovim’s mechanism for virtual text / decoration) to apply highlighting, with a default priority so that other highlights can override or be overridden.
* **Fallback**: If a capture is very specific (e.g. `@comment.documentation`) but not defined in your colorscheme, Tree‑sitter falls back to more general groups (e.g. `@comment`).

### File Locations and Runtime Structure

* **Parsers**: As mentioned, stored in `parser/{lang}` under any directory in `runtimepath`. Neovim will pick the first matching parser.
* **Queries**: `.scm` files for queries go in `queries/{language}/`, e.g., `queries/lua/highlights.scm`.
* **Registration**: You can manually register a parser or map filetypes to parser names using `vim.treesitter.language.add()` and `vim.treesitter.language.register()`.

### Tree-sitter Benefits

* **Accuracy**: Because it sees the real syntax tree, highlighting is far more precise and resilient than regex.
* **Context‑sensitivity**: Nested or context‑sensitive constructs (e.g., interpolated strings, embedded languages) are handled naturally because they exist in the tree.
* **Performance**: Incremental parsing means only changed regions are re-parsed, so large files can remain responsive.
* **Extensibility**: You can write or customize your own `.scm` queries to highlight exactly what you care about (parameters vs locals, doc comments vs inline comments, etc.).

---

## `nvim-treesitter`

**`nvim-treesitter`** is a Neovim plugin that makes managing Tree‑sitter parsers and query files much easier.

* Rather than manually compiling parsers and placing `.so` files in `runtimepath`, `nvim-treesitter` provides commands to **install**, **update**, and **uninstall** parsers.
* It also bundles query files (`highlights.scm`) for many languages, so you don’t have to write them from scratch.
* With `nvim-treesitter`, you can *enable or disable* modules (like highlighting) per language or buffer, giving you fine-grained control.
* Importantly, `nvim-treesitter` manages versioning: it ensures the parser version matches what the plugin expects.

---
