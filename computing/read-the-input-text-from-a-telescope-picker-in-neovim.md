# Read the input text from a Telescope picker in NeoVim

#neovim #telescope

-----

To read the text typed by the user in the current Telescope picker, 
retrieve the state of the current picker given the prompt buffer number `prompt_bufrn`
and then use the function `_get_prompt` to read the input value.

```lua
local picker = require('telescope.actions.state').get_current_picker(prompt_bufnr)
if picker then
    local title = picker:_get_prompt()
    print(title) -- print the input text
end
```
