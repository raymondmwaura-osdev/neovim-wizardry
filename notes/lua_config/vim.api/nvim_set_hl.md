# `vim.api.nvim_set_hl`

## Overview

`vim.api.nvim_set_hl` is a function in Neovim’s Lua API that allows a developer to define or overwrite a highlight group. It operates on the *editor’s UI state* (specifically the global or namespaced highlight definitions that control how text and interface elements are displayed). Developers call it when they want to programmatically set colors/styles (foreground, background, bold, underline, links, etc.) for syntax groups, UI elements, or custom highlight groups; for instance when building a colorscheme, customizing UI, or dynamically adjusting styles in a plugin.

---

## Signature

* **Lua:** `vim.api.nvim_set_hl(ns_id, name, val)`
* **API Name:** `nvim_set_hl`
* **Since:** 0.5.0

---

## Parameters

* `ns_id`; integer (required). A namespace identifier (as returned by `nvim_create_namespace()`). Use `0` to apply the highlight group globally (i.e. equivalent to Vim’s `:highlight`). Namespaces other than 0 allow grouping highlights so they can later be activated per-window with `nvim_set_hl_ns()` or `nvim_win_set_hl_ns()`.
* `name`; string (required). The name of the highlight group to define or overwrite (e.g., `"Normal"`, `"Comment"`, or a custom group name).
* `val`; table (required). A map describing the highlight attributes. The table may include these keys (among others):

  * `fg`, `bg`, `sp`: strings (color names or `"#RRGGBB"`). `fg` = foreground, `bg` = background, `sp` = special (e.g. undercurl color).
  * `blend`: integer between 0 and 100 (transparency / blending)
  * Boolean style flags: e.g. `bold`, `underline`, `undercurl`, `underdotted`, `underdashed`, `underdouble`, `strikethrough`, `italic`, `reverse`, `standout`, `nocombine`.
  * `ctermfg`, `ctermbg`: integers or names for terminal-color fallback (for color terminals)
  * `cterm`: a sub-table to configure terminal-specific attributes if needed.
  * `link`: string (the name of another highlight group; this causes the named group to simply link to that group (similar to `:highlight link`)) when `link` is present, other style/colour keys are ignored.
  * `default`: boolean; if true, will only set the highlight if the group does not already have a definition (i.e. do not override existing)
  * `force`: boolean; if true, force update the highlight group even if it already exists

---

## Returns

* Return type: `nil`.
* The function does not return any structured value or error object. On success, the highlight definition is applied. On failure (e.g. invalid parameters), Neovim will raise an error (i.e. Lua error).

---

## Behavior

When invoked, `nvim_set_hl` replaces (or defines) globally (or namespaced) the highlight group named `name` with the attributes described in `val`. If previously that group existed, the old definition is overwritten; this is a full replacement, not a merge or patch. For instance, calling `nvim_set_hl(0, 'Visual', {})` will clear the definition for `Visual`.

Internally, Neovim updates its highlight-group registry (either global or within the specified namespace), which affects rendering for any buffers/windows using that highlight group. The UI will reflect the change on next redraw (generally immediately). The call does not itself trigger buffer-local autocommands (unless other parts of the code depend on highlight-changes). The state change is global (or namespace-level), not buffer-local; it does not depend on a particular buffer or window. Because highlight groups are part of UI styling, this does not affect file contents or buffer state; only presentation.

If `ns_id` ≠ 0, the highlight is only active if the given namespace is made active for a window via `nvim_set_hl_ns()` or `nvim_win_set_hl_ns()`. Otherwise those namespace-specific highlights will not apply.

If `val` includes `link`, then the group becomes a link: it points to another highlight group, and only the link is honored (other styling keys are ignored), analogous to Vim’s `:highlight link`.

---

## Usage

### Example: Basic

```lua
vim.api.nvim_set_hl(0, "Comment", { fg = "#FF0000", italic = true })
```

That sets the `Comment` highlight group globally to red-colored, italic text.

### Example: Real-world Integration

In a colorscheme plugin’s Lua setup:

```lua
-- after applying base colorscheme
vim.api.nvim_set_hl(0, "LineNr", { fg = "#5eacd3", bold = true })
vim.api.nvim_set_hl(0, "CursorLineNr", { fg = "#ffd700", bold = true })

-- ensure custom overrides persist when user changes colorscheme
vim.api.nvim_create_autocmd("ColorScheme", {
  callback = function()
    vim.api.nvim_set_hl(0, "LineNr", { fg = "#5eacd3", bold = true })
    vim.api.nvim_set_hl(0, "CursorLineNr", { fg = "#ffd700", bold = true })
  end,
  desc = "Restore custom line-number colors after colorscheme change",
})
```

Or in a plugin that uses namespace-specific highlighting:

```lua
local ns = vim.api.nvim_create_namespace("MyPluginMarks")
-- define highlight group within namespace
vim.api.nvim_set_hl(ns, "MyPluginMark", { bg = "#203040", fg = "#ffffff", bold = true })
-- later, for each window you set:
vim.api.nvim_win_set_hl_ns(win_id, ns)
-- now highlights will show in that window using "MyPluginMark"
```

---

## Errors and Edge Cases

* If `ns_id` is invalid (e.g. not created via `nvim_create_namespace()` or not an integer), or `name` is not a string, or `val` is malformed (e.g. wrong types, invalid color string, out-of-range `blend`, unknown keys), Neovim will raise a Lua error.
* Because the function *replaces* highlight definitions, any existing attributes not specified in `val` are lost. This is a common footgun: if you intend to only modify one attribute (e.g. `fg`), you may inadvertently wipe out other attributes (e.g. background, underline).
* Using `link` with other attributes: the API ignores non-`link` attributes if `link` is present; so specifying both `link = "OtherGroup"` and, say, `fg = "#ff0000"` will result in only the link taking effect, and colors being ignored.
* If using non-global namespace (ns_id ≠ 0) but never activating that namespace via `nvim_set_hl_ns()` or window-specific `nvim_win_set_hl_ns()`, the definitions will have no visible effect.
* When using `default = true`, the highlight is only applied if the group does not yet exist; if the group is already defined (e.g. by a colorscheme), `default = true` will result in no change.
* Interactions with external factors: a later colorscheme load or plugin may overwrite your highlight. In practice, you often need to place `nvim_set_hl` calls in a `ColorScheme` autocmd or after plugin/theme loading.

---

## Testing / Verification

To verify that `nvim_set_hl` worked:

1. In a Neovim session (with `termguicolors = true` if using hex colors), call:

   ```lua
   vim.api.nvim_set_hl(0, "TestGroup", { fg = "#00ff00", bg = "#000000", bold = true })
   ```
2. Use a command or insert text, then apply the highlight group by e.g. syntax match or manual assignment, or link existing group:

   ```vim
   :highlight link CursorLine TestGroup
   ```

   or if you just want to observe UI effect, examine statusline or interface elements that use that group, or set `guifg=TestGroup` on some element.
3. Alternatively, inspect the group definition via:

   ```lua
   print(vim.inspect(vim.api.nvim_get_hl_by_name("TestGroup", true)))
   ```

   Ensure the returned table includes the keys `fg`, `bg`, `bold`, etc., matching your expectation.
4. In a plugin’s test suite: programmatically call `nvim_set_hl`, then call `nvim_get_hl_by_name` and assert the resulting table matches the expected attributes.

If using namespace-based highlights, ensure that after defining you call `nvim_set_hl_ns(...)` or `nvim_win_set_hl_ns(...)`, then check the visual result in that window.

---
