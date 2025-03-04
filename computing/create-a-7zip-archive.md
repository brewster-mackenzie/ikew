# Create a 7zip archive

#7zip #compression #io #filesystem

-----

## Add command

Recursively add files and folders from `foldername` to archive `archive.7z`

Filenames will be prefixed with `subdir/`

```bash 
7z a archive.zip foldername\
```

Recursively add files and folders from `foldername` to archive `archive.7z`

Filenames will **not** be prefixed with `subdir/`

```bash
7z a archive.zip .\foldername\*
```

Add all files with the `.txt` extension from the current folder to archive `archive.7z`

```bash 
7z a archive.7z *.txt
```

Recursively add all files with the `.txt` extension to archive `archive.7z`

```bash 
7z a archive.7z *.txt -r
```

Combine with other options, such as `-t` to set the archive type, `-p` to set password
encryption, etc.
