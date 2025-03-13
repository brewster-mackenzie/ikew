# Integrate neovim status into the tmux statusline

#neovim #tmux 

-----

To achieve this I use `vim-tpipeline`, a vim plugin that just so happens to work in neovim too.

Add this to `~/.config/nvim/init.lua` or another Lua script

```lua
use {
  'vimpostor/vim-tpipeline',
}
```

If you use another script file, make sure to include it in `init.lua`

Finally set the `cmdheight` in neovim to `0`

```vim
vim.cmd[[set cmdheight=0]]
```

And restart neovim to apply the changes.

It is also recommended to update `~/.tmux.conf` as follows:

```bash
set -g focus-events on
set -g status-style bg=default
set -g status-left-length 90
set -g status-right-length 90
set -g status-justify absolute-centre
```

And then reload tmux config (default `^I`) to apply the changes.
