# Tokyo Night theme

#theming #ui

-----

TODO: add full palette 


## neovim 

Use [folke/tokyonight.nvim](https://github.com/folke/tokyonight.nvim)

Install the plugin with your plugin manager of choice (example below is Packer)

```lua 
return require('packer').startup(function(use)
  use 'folke/tokyonight.nvim'
end)
```

Then initialize the plugin:

```lua
require"tokyonight".setup{
        style="storm"
}

vim.cmd[[colorscheme tokyonight-night]]
```


## tmux

Use [fabioluciano/tokyo-night-tmux](https://github.com/fabioluciano/tmux-tokyo-night)


Add to `.tmux.conf` like this: 

```bash
set -g @plugin "fabioluciano/tokyo-night-tmux"
set -g @theme_variation 'storm'
```
