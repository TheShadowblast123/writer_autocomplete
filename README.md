# Writer Completion in neovim

## Manual Completion

### But wait this says autocomplete....

Yes, but the point of this section is to inform you about manual completion systems so that you have extra tools at your disposal for lessening the amount of typing that you have to do and the more executing on your vision you get to do.
### So what's the good stuff

Hopefully if you're reading an article or a readme on github about neovim, you're already fairly situated with how exactly it works, but just in case here's the relevant mini refresher.
In order to type new text in vim, you must type "i". This is because by default you enter a document/file in normal mode, which is a mode for editing, not typing new text. This in a way is a great thing as it divides editing from writing, but for completion, the best part is that everything that'll make us faster is in insert mode. It's in insert mode so much so that's it is called...

### Insert mode completion

In insert mode "Ctrl x" (that is the combination of the control/command key and the "x" key) puts you in "ins-completion" which gives you all sorts of completion power. The following commands are those most relevant to writers, which means, yes there are many missing, and no I do not care programmer that you came to a writing github/article to learn programming.

#### All of the following require "Ctrl x" and take in what has already been typed as a prefix:

- "Ctrl l" (l as in label) whole line completion. Useful for poetry, or paragraphs that happen to be heavily correlated (perhaps in a story, a character is going through a loop or revisiting somewhere that has changed but is still the same in certain ways)
- "Ctrl n" words in current file, useful for isolating the current file's words for suggestions
- "Ctrl r" registers. With very smart usage possibly the most powerful, though often it will be unused.

### Example usage

*NOTE*: **if there's more than one match for a completion, it'll show you the list of matches, but if there's one unique match, it will just insert that for you. Also as with other commands in vim, you don't need to release ctrl instead doing hold ctrl press x press l as in label**

#### Line Completion: "Ctrl x" + "Ctrl l"

![nvim exe2025-08-1101-16-50-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/f7fa0a87-f196-464e-b6a9-2f4e9d593d6a)

This is particularly useful for anything with heavy reppetion or something that happens to share a structure that is easier to edit than it is to retype.

#### Current File Word Completion: "Ctrl x" + "Ctrl n"

![nvim exe2025-08-1110-08-25-ezgif com-optimize](https://github.com/user-attachments/assets/92aee872-0db8-46f5-898a-4b758f5d9b7a)

This is one of the weaker forms of manual completion, as any good autocomplete pratically provides this functionality with less key presses, but they won't start using the context of the completion for extra completions. This is highly situational when you consider autocomplete.

#### "Ctrl x" + "Ctrl r"

![ctrlr-ezgif com-video-to-gif-converter](https://github.com/user-attachments/assets/93673f46-efa8-4a5f-a214-3f23969f51c8)

This is the most tactical completion. It is only as useful as you are willing to remember it and what you did around it. Pro tip: . is a register for the last thing typed and (letter)y will yank to the register "letter" (example: "ayw, into register <ins>a</ins> <ins>y</ins>ank <ins>w</ins>ord )

### Final word on manual completion

Pressing three characters is shorter than most words. Pressing three characters is shorter than most lines. Pressing 4 characters can be shorter than typing something that comes up regularly. Vim and In my focus, NeoVim help you edit faster but with completion, they also artificially increase your typing speed. Google Docs doesn't do that. Word doesn't do that. Most writing programs won't do that and we're about to get an extra speed boost though automation.

## Autocomplete (finally)

### WPM and it's underlooked cousin, CPS

At this point we have to have a discussion. Let's assume that the average word is 4 characters long for the sake of easier math. If you type 30wpm that would be 120cpm (characters per minute) or 2cps (characters per second). Let's say I give you a list of options, how far from the first option could a big word be and still make sense as an option? You'd have to be typing the word, looking at the screen, make the decision and go for it. The word would have to either be a tricky word or so long that going down a list of options is fine for getting to it. Now let's say you type at 120wpm, this now means you type at 8cps. What would work for someone at 2cps won't work for you. 

But let's say we did find something that did work for you and let's call this number x. We're going to make the assumption that we will go with an amount of suggestions that doesn't meaningfully impact reaction times. It will always take 2 character presses at least in order to get any suggestion, This means s<sub>1</sub> must be x + 2, where x is the amount of characters you could type you could reasonably type before realizing the suggestion is exactly what you want. Any suggestion further down must generally follow an additional +1 in length. Now of course there will be instances where you want difficult to spell words, but they too should fall, generally, in this order.

Here are my suggestions as far as minimum suggestion lengths:
- 1 or 2 character(s): Don't use neovim outside of editing. Don't even type. You're probably faster writing by hand if this is actually useful. You'd have to type at at most 15 wpm and a whopping 1 character per second... but that means autocomplete doesn't save any time
- 3 characters: Occasionally justifiable at 30wpm.
- 4 characters: 30wpm is justified, 45 wpm is somewhat justified.
- 5 characters: 45 wpm is justified, but now the issue is the possibility of just wanting an autocomplete to type words correctly goes up. Even at 90% accuracy there's about a 50% chance of typing a word incorrectly.
- 6+ characters: Now at 92% accuracy, there's a 48% of typing a word these lengths incorrectly. Part of the speed bonus of autocomplete is being assured that you're typing a word correctly.
That's minimum length of the suggested word, I then also recommend a minimum of minimum length + 2 for prefixed suggestions. Here's my recommendations:
- 2 characters: good for 30wpm
- 3 characters: good for 60wpm
- 4+ characters: good for 90+wpm but again, requires higher accuracy

  So with this information, here's my recommended autocomplete setup for writers with lazynvim
``` lua

{
	"TheShadowblast123/cmp-adaptive-freq",
	dependencies = { "hrsh7th/nvim-cmp" },
	config = function()
		require("cmp-adaptive-freq").setup({})
	end,
},
{
	"TheShadowblast123/cmp-freq",
	dependencies = { "hrsh7th/nvim-cmp" },
	config = function()
		require("cmp-freq").setup()
	end,
},
{
	"hrsh7th/nvim-cmp",
	event = "InsertEnter",
	dependencies = {
		"hrsh7th/cmp-path", -- source for file system paths
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
				["<Tab>"] = cmp.mapping.confirm({ select = false }), -- confirm with tab
			}),
			-- sources for autocompletion
			sources = cmp.config.sources({
				{ name = "cmp-adaptive-freq" }, -- personalized word frequency
				{
				name = "cmp-freq",
				entry_filter = function(entry, ctx)
					local word = entry:get_insert_text()
					return (#word >= 3) and (#word >= #ctx.context.cursor_before_line + 2)
				end
				}, -- generealized word frequency
				{ name = 'path' }, -- just for file paths
			}),
		})
	end,
}
```
So first the completion sources need explaining. "cmp-freq" is a completion source based on word frequency lists with multi language support and even custom language support, if you're willing to make a word frequency list that is. This is used to give general word suggestions and assure that for the most part, words are spelled correctly the first time they're typed, but later is where cmp-adaptive-freq shines. It is a completion source that adapts to the user's input within a session, within a folder, and globally. It's about as good as an autocomplete could get without getting very complicated or invasive. "path" is specifically for 
The keymappings are relatively unimportant though it pains me to say that tab is the superior confirmation key. It's rare to use tab in the middle of a line but ending a paragraph is just part of the buisness.
