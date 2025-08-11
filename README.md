# Writer Completion in neovim

## Manual Completion

### But wait this says autocomplete....

Yes, but the point of this section is to inform you about manual completion systems so that you have extra tools at your disposal for lessening the amount of typing that you have to do and the more executing on your vision you get to do.
### So what's the good stuff

Hopefully if you're reading an aritcle or a readme on github about neovim, you're already fairly situated with how exactly it works, but just in case here's the relevant mini refersher.
In order to type new text in vim, you must type "i". This is because by default you enter a document/file in normal mode, which is a mode for editing, not typing new text. This in a way is a great thing as it divides editing from writing, but for complettion, the best part is that everything that'll make us faster is in insert mode. It's in insert mode so much so that's it is called...

### Insert mode completion

In insert mode "Ctrl x" (that is the combination of the control/command key and the "x" key) puts you in "ins-completion" which gives you all sorts of completion power. The following commands are those most relevant to writers, which means, yes there are many missing, and no I do not care programmer that you came to a writing github/article to learn programming.

#### All of the following require "Ctrl x" and take in what has already been typed as a prefix:

- "Ctrl l" (l as in label) whole line completion. Useful for poetry, or paragraphs that happen to be heavily correlated (perhaps in a story, a character is going through a loop or revisiting somewhere that has changed but is still the same in certain ways)
- "Ctrl n" words in current file, useful for isolating the current file's words for suggestions
- "Ctrl i" words in current and "included" files. This is good for including extra files for suggestions
- "Ctrl r" registers. With very smart usage possibly the most powerful, though often it will be unused.

### Example usage

#### Line Completion: "Ctrl x" + "Ctrl l"

1. Pantoum

#### Current File Word Completion: "Ctrl x" + "Ctrl n"



#### "Ctrl x" + "Ctrl i"



#### "Ctrl x" + "Ctrl r"



## Autocomplete (finally)




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
