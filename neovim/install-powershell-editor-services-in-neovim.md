# Install PowerShell editor services in NeoVim

Ensure that [[install-mason-in-neovim|Mason is installed first.]]

Then install `powershell-editor-services`

```
:MasonInstall powershell-editor-services
```

And include it in your Mason setup:

```lua
require('mason').setup()
require('mason-lspconfig').setup({
  ensure_installed = {
    "powershell_es", -- include this line
  }
})
```

Finally add the `powershell_es` initialisation into your `init.lua` or another script.

You might need to adjust the `bundle_path`

In my case the files are in `~/.local/share/nvim`

```lua
require"lspconfig".powershell_es.setup { 
  bundle_path = vim.fn.stdpath("data") .. "/mason/packages/powershell-editor-services",
  filetypes = { "ps1", "psm1", "psd1" },
}
```
