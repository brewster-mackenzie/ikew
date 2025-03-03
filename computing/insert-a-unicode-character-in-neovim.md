# Insert a unicode character in NeoVim

#neovim #unicode #computing

-----

Characters with a hex code of 4 digits or less, type
`<C-v>uffff` where `ffff` is the hex code.

For example, to type the middle dot symbol `·` which is 
code `00b7` you would enter:

```vim
<C-v>u00b7
```

Or the shorter version:

```vim
<C-v>ub7
```

Characters with a hex code of 5-8 digits, type `<C-v>Uffffffff`
using the uppercase `U` instead.

For example to type the grinning cat emoji `😺` which is code
`1f63a` you would enter:

```vim
<C-v>U0001f63a
```

Or the shorter version:

```vim
<C-v>U1f63a
```


