# Install Mason in NeoVim

Ensure the following packages are loaded using the package manager of your choice:

```
williamboman/mason.nvim
williamboman/mason-lspconfig.nvim
neovim/nvim-lspconfig
```

For example using Packer:

```lua
return require('packer').startup(function(use)
  use 'williamboman/mason.nvim'
  use 'williamboman/mason-lspconfig.nvim'
  use 'neovim/nvim-lspconfig'
end)
```

And ensure that it is initialised in `init.lua` or another script:

```lua
require('mason').setup()
require('mason-lspconfig').setup({
  ensure_installed = {
    -- you will put your required servers here
  }
})
```

