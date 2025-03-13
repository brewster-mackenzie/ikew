# Delete the current file using Neovim

#neovim 

-----


Using the current file `%` register 

```vim
:call delete(@%)
```

To also purge the current buffer

```vim
call delete(@%) | bdelete!
```

Map deletion of the current file to `<leader>rm`

```vim
map('n', '<leader>rm', [[:call delete(@%)<CR>]], { noremap=true, silent=true })
```

Map deletion and purge of the current file to `<leader>rM`

```vim
map('n', '<leader>rM', [[:call delete(@%) | bdelete!<CR>]], { noremap=true, silent=true })
```


