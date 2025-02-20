# Managing dotfiles with Git

Instructions for managing your dotfiles with Git.  Works in Linux and Windows.


## Steps 

### Checkout bare repository

```bash
git clone --bare <remote-git-repo-url> <checkout-dir>
```

- replace `<remote-git-repo-url>` with the repository URL
- replace `<checkout-dir>` witht he checkout location (NOT the home folder)

### Add alias to Git global config

```bash
git config --global alias.dotfiles '!git --git-dir=<checkout-dir> --work-tree=<home-dir>'
```

- replace `<checkout-dir>` with the chckout location
- replace `<home-dir>` with the home folder (e.g. `/home/brewster` or `c:/users/brewster`)

### Checkout

```
git dotfiles checkout
```

### Next steps 

Create a branch for the new machine

```bash
git dotfiles checkout -b 202409-new-machine-name
```

Make changes specifically for this new-machine-name

```bash
git dotfiles add ~/.config/app/app.config

git dotfiles commit -m "added new app config file"
git dotfiles push
```

If changes are applicable to all environments, merge into main via a feature branch with a single commit.


## Windows example

Make sure to use forward-slashes in alias paths:

```bash
git clone --bare https://github.com/myname/dotfiles C:\Users\myname\repos\dotfiles

git config --global alias.dotfiles '!git --git-dir=c:/users/myname/repos/dotfiles --work-tree=c:/users/myname'

git dotfiles checkout
```

