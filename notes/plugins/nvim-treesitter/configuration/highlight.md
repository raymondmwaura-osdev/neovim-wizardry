# Highlight Module

## Conceptual Overview

The highlight module orchestrates a multi-layered rendering pipeline that maps syntactic nodes from Tree-Sitter’s abstract syntax trees to Neovim’s internal highlight groups. Unlike legacy Vim regex-based syntax engines, Tree-Sitter operates through a fully incremental parsing graph, allowing the highlighting system to maintain deterministic correctness at scale.

The configuration table for the module governs three primary axes of behavior:

1. **Activation** of the highlight subsystem
2. **Governance** of conditional enable/disable rules per language
3. **Interoperability** with Neovim’s native regex highlighter

Each option modifies runtime execution flow and performance characteristics.

---

## Option: `enable`

### Semantic Role

`enable` is a boolean flag indicating whether the Tree-Sitter highlight engine should be initialized and attached to buffers matching installed parsers.

### Operational Dynamics

* When `true`, Tree-Sitter attaches a highlighting client per buffer, synchronizes the parse tree, and derives highlight groups based on the active grammar.
* When `false`, the highlight module is entirely bypassed, eliminating its CPU footprint.

### Strategic Considerations

Use `enable = true` unless you have a targeted reason to disable the entire subsystem (e.g., performance testing, benchmarking legacy Vim syntax, or debugging custom Treesitter queries).

---

## Option: `disable`

### Semantic Role

`disable` controls conditional suppression of highlighting for specific languages or contexts. It accepts either:

1. A **list** of parser names,
2. A **function(buffer, parser)** returning `true` or `false`.

### Operational Dynamics

If the language of the opened buffer matches the disable rule, the Tree-Sitter client is never attached to the buffer.

### Practical Use Cases

* **Performance risk mitigation:** disabling expensive grammars such as `markdown`, `org`, or extremely large file types.
* **Context-aware control:**

  ```lua
  disable = function(lang, buf)
    local max = 200000
    return vim.api.nvim_buf_line_count(buf) > max
  end
  ```

  This pattern is commonly adopted to avoid parsing large buffers.

### Strategic Guidance

In a corporate engineering environment, deploying a conditional disable function is usually a best-practice to maintain predictable SLA performance during large-file workflows.

---

## Option: `additional_vim_regex_highlighting`

### Semantic Role

This flag determines whether Neovim’s legacy regex-driven syntax engine should coexist with Tree-Sitter highlighting.

It accepts either:

1. A **boolean**, applying globally, or
2. A **list of languages**, enabling dual-highlighting selectively.

### Operational Dynamics

* When set to `true`, both engines run concurrently.
* Neovim merges highlight groups from both sources; conflicts are resolved by standard priority rules.

### Implications

Concurrent execution increases CPU and memory utilization, and it may cause “highlight flicker” or redundancy in certain grammars.

### Recommended Usage

* Set `false` in most operating models.
* Override only for languages where Vim’s regex engine provides indispensable auxiliary semantics (e.g., indentation logic for some legacy grammars, highlighting of embedded templating languages, or compatibility with legacy plugins).

### Example

```lua
additional_vim_regex_highlighting = { "markdown" }
```

This hybrid deployment model is frequently found in enterprise-grade configurations that need to preserve intricate semantic highlighting for Markdown front-matter, fenced code blocks, or plugin-driven syntax overlays.

---
