# Bash snippets

Rename and remove *underscore* from files in current folder

```bash
for f in _*; do mv -v "$f" $(echo "$f" | tr -d '_'); done
```

Generate *html* file from *RTF* files based on their basename and content

```bash
for f in *.rtf; do echo -E "$(basename -s .rtf "## $f") $(unrtf --quiet "$f")" >> index.html; done
```

Find and print all files starting with ".DS_" 

```bash
find -type f -name ".DS_*" -print
```

Find all file occurences starting with "._" and delete them 

```bash
find -type f -name "._*" -delete
```

Find folder and delete its content

```bash 
find -type d -name "foldername" -exec rm -rf {} \;
```

Display file permission bits in octal

```bash
stat -c "%a %n" <filename>
```

