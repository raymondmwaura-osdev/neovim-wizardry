# Keycodes

## What are keycodes / special key notation

* The `<…>` strings (e.g. `"<CR>"`, `"<Esc>"`, `"<C-x>"`, etc.) are symbolic representations of *special keys* or *key combinations*; meaning keys that **do not correspond to a simple printable character**.
* In the context of mapping (or remapping) keys in Vim/Neovim, they allow you to bind or trigger keys like Enter, Escape, function keys, arrows, and modified keys (Ctrl, Alt/Meta, Shift, etc.) in a readable and consistent format; instead of embedding raw control characters or weird escape sequences.
* Under the hood, the editor (or terminal) receives control codes (or escape sequences) for these special keys. The `<…>` notation is a human‑friendly abstraction over those codes. For example, `<CR>` corresponds to the “carriage return / Enter” key. `<Esc>` corresponds to the Escape key, etc.
* Because different environments (terminals, OS, GUI, etc.) may have different ways of transmitting key presses, not all possible combinations will work everywhere; Vim/Neovim only recognizes keys that the environment supports.
* The official documentation refers you to the help topic `:help key-notation` (or `:help keycodes`) for a full reference list.

---

## What they are used for

* **Defining key mappings**: When you want a certain key (or key combination) to trigger a command, you use these codes so Vim/Neovim knows exactly which key(s) you mean. Example: mapping `<Leader>e` to `:Ex<CR>`; where `<CR>` presses Enter to execute the command.
* **Representing non‑printable keys**: Keys like Escape, Tab, Delete, Home, arrow keys, function keys, etc., all need some symbolic way to be referred to; this notation fills that role.
* **Modifier combinations**: You can combine modifiers (Ctrl, Shift, Alt/Meta, etc.) with other keys in a unified syntax like `<C-…>`, `<S-…>`, `<M-…>` (or `<A-…>`); making complex shortcuts clear and maintainable.
* **Portability and readability**: Instead of embedding obscure escape sequences (which could vary between terminals or OSes), this abstract notation ensures your config remains readable, shareable, and consistent across environments.
* **Escaping problematic characters**: Because `<…>` notation handles special cases (e.g. representing a literal `<`, or using backslashes) in a canonical way, you avoid confusion or conflicts in mappings.

---

## Common Key Notation Codes

Here is a (nearly) comprehensive list of the common special key codes you are likely to use or encounter.

* `<CR>`; Enter / Return (carriage return)
* `<Enter>`; same as Enter / Return
* `<Return>`; same as Enter / Return
* `<Esc>`; Escape key
* `<Tab>`; Tab key
* `<Space>`; Spacebar
* `<BS>`; Backspace
* `<Up>`; Up arrow key
* `<Down>`; Down arrow key
* `<Left>`; Left arrow key
* `<Right>`; Right arrow key
* `<F1>` … `<F12>`; Function keys F1 through F12
* `<Insert>` (or `<Ins>`); Insert key
* `<Del>` (or `<Delete>`); Delete key
* `<Home>`; Home key
* `<End>`; End key
* `<PageUp>`; Page Up key
* `<PageDown>`; Page Down key
* `<bar>`; The literal ‘|’ character (useful in mappings where ‘|’ has special meaning)
* `<lt>`; Literal “<” (less‑than) sign (when you need a literal `<` in a mapping)

### Modifier‑prefixed / Combined Keys

* `<C-…>`; Ctrl + (some key), e.g. `<C-R>` for Ctrl‑R, `<C-Space>` for Ctrl + Space.
* `<S-…>`; Shift + (some key), e.g. `<S-F2>` for Shift + F2.
* `<M-…>` or `<A-…>`; Meta / Alt + (some key). On many systems, Alt is used; in some contexts “Meta” or “Alt/Meta” is equivalent.
* Combined modifiers; you may combine multiple modifiers, e.g. `<C-S-F3>` (Ctrl + Shift + F3), or `<C-A-…>`, etc.

---

## Important Caveats and Additional Notes

* Not all key combinations are always distinguishable. For example early on `<CR>` was effectively the same as `<C‑M>` (Ctrl‑M), `<Tab>` was often equivalent to `<C-I>`. In later versions (for example in some GUI or terminal emulators) they may be distinguished; but that depends on whether the environment sends distinct codes.
* For mappings involving very literal characters (like `<`, `\`, or `|`) there are special notations (`<lt>`, `<Bslash>`, `<bar>`, etc.) to avoid conflicts or parsing ambiguity.
* If using a terminal emulator, the terminal must support sending the key codes for the special/modified keys; otherwise Vim/Neovim will not “see” them.

---
