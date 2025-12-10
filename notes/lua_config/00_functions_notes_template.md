# <FUNCTION NAME>

## Overview

Explain the purpose of the function in 2-3 sentences.  
Clarify *what area of Neovim it operates on* (buffer, window, UI, state), *why it exists*, and *when a developer would call it*.  
Keep this high-level but concrete.

---

## Signature

Show exactly how the function is invoked.

- **Lua:** `vim.api.<function_name>(...)`  
- **API Name:** `nvim_<function_name>`  
- **Since:** <version>

Include signature only; no commentary.

---

## Parameters

For each parameter, describe the essentials (name, type, required, description) (don't use a table):

Keep this section tight: definition, expectation, constraints.  
Do not explain behavior here; only *what the function expects*.

---

## Returns

Describe exactly what the function gives back:

- Return type (string, number, table, handle, nil)  
- Structure of the table if applicable  
- Meaning of each returned element  
- Whether error states return nil or raise an exception  

---

## Behavior

Explain how Neovim processes the call.

Cover only the functional mechanics:

- What the function does internally  
- What editor state it changes  
- Whether it triggers autocommands  
- Whether it updates UI immediately or schedules updates  
- Whether it interacts with buffers, windows, or global state  

---

## Usage

Give two example patterns:

### Example: Basic  

A minimal line showing typical usage.

### Example: Real-world Integration  

A short scenario showing how developers normally apply it:
- inside autocmds  
- inside plugin code  
- with callbacks  
- chaining related functions  

---

## Errors and Edge Cases

Document failure conditions:

- Invalid buffers/windows  
- Out-of-range values  
- Missing fields  
- Operation silently ignored  
- Known footguns  

---

## Related APIs

Link functions that support, extend, or complement this one.

- Functions that perform the inverse operation  
- Higher-level wrappers  
- Older or Vimscript equivalents  

---

## Testing / Verification

Show minimal verification steps:

- How to inspect outputs  
- How to confirm state changes (e.g., `vim.api.nvim_get_*` functions)  
- How to assert success in plugin tests  

---
