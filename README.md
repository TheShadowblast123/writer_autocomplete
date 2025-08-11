# Writer Completetion in neovim
## Requirements:
- touch typing capabilities
- some familiarity with vim key bindings
- an open mind
## Benefitis:
- faster typing
- TO BE CONTINUED




A simple .md containing information and implementations of autocomplete systems for neovim
``` lua
return {
  "hrsh7th/nvim-cmp",
  event = "InsertEnter",
  dependencies = {
    "hrsh7th/cmp-buffer", -- source for text in buffer
	-- here would be cmp-spell
	-- here would be cmp-freq
	-- here would be cmp-peronalized-freq
  },
  config = function()
    local cmp = require("cmp")
    cmp.setup({
      completion = {
        completeopt = "menu,menuone,preview,noselect",
      },

      mapping = cmp.mapping.preset.insert({
        ["<C-k>"] = cmp.mapping.select_prev_item(), -- previous suggestion
        ["<C-j>"] = cmp.mapping.select_next_item(), -- next suggestion
        ["<C-Space>"] = cmp.mapping.complete(), -- show completion suggestions,
        ["<C-e>"] = cmp.mapping.abort(), -- close completion window
        ["<CR>"] = cmp.mapping.confirm({ select = false }), -- confirm with enter
      }),
      -- sources for autocompletion
      sources = cmp.config.sources({
        { name = "buffer" }, -- text within current buffer
      }),
    })
  end,
}
```
test test test
