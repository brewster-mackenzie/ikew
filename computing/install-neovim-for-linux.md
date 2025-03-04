# Install NeoVim for Linux

#neovim #tar #curl #grep #aws #tr #sed 

-----

Download from the official GitHub repository

Get release version from the `location` header at `releases/latest`

Make sure to trim the trailing whitepsace using `tr` afterwards

Use this to construct a download URL and save it to a local file


```bash
curl -LO $(curl -Is https://github.com/neovim/neovim/releases/latest | grep -i location | awk '{print $2}' | sed 's/tag/download/g' | tr -d '\r\n')"/nvim-linux-x86_64.tar.gz"
```

Extract the tarball

```bash
tar xzvf nvim-linux-x86_64.tar.gz
```

Move the files to a location and symlink the binary

```bash 
sudo mv nvim-linux-x86_64 /usr/local/bin # or whatever location you prefer
sudo ln -s /usr/local/bin/nvim-linux-x86_64/bin/nvim /usr/local/bin/nvim
```

Full example

```bash
curl -LO $(curl -Is https://github.com/neovim/neovim/releases/latest | grep -i location | awk '{print $2}' | sed 's/tag/download/g' | tr -d '\r\n')"/nvim-linux-x86_64.tar.gz"
tar xzvf nvim-linux-x86_64.tar.gz
sudo mv nvim-linux-x86_64 /usr/local/bin # or whatever location you prefer
sudo ln -s /usr/local/bin/nvim-linux-x86_64/bin/nvim /usr/local/bin/nvim
```

