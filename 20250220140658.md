# Create a symbolic link

## Linux


```bash
ln -s <target> <source>
```

## Windows

### PowerShell

File symbolic link

```powershell
New-Item $link -ItemType SymbolicLink -Value $target 
```

Directory link using mklink which is required for 
some target object types.  This example ensures that the 
link path and target directory path are quoted.

```powershell
cmd /c mklink /D "`"$link`"" "`"$target`""
```


