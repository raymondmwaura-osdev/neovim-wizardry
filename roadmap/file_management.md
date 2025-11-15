## Learning Roadmap (Progressive Levels)

### Level 1: Baseline Navigation

* Open files: `nvim <file>`, `:edit`.
* Move between buffers: `:ls`, `:b <id>`.
* Write/quit: `:w`, `:q`, `:wq`.

### Level 2: Operational Efficiency

* Window management: `:split`, `:vsplit`, `<C-w>` motions.
* Buffers as first-class objects: `:bdelete`, `:bnext`, `:bprev`.
* Directory jumps: `:cd`, `:lcd`, `:pwd`.

### Level 3: Native File Operations

* Create files by editing non-existent paths.
* Shell passthrough for deletes/moves: `:!rm`, `:!mv`, `:!cp`.
* NetRW (built-in): open with `:Ex` or `:Lexplore` for basic file browsing.

### Level 4: Plugin-Driven File Management

* *nvim-tree* for tree-view navigation and CRUD.
* *telescope-file-browser* for fuzzy-driven directory operations.
* *oil.nvim* for directory-as-buffer, inline file editing.

### Level 5: Enterprise-Grade Workflow Optimization

* Custom keymaps for create/delete/rename flows.
* Auto-commands to sync working directory with the current file.
* Integrated search + refactor pipelines across directories.
* Git-aware file management via *telescope*, *oil*, or *lazygit* integration.

---
