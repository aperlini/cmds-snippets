# Commands Snippets

> commands history : things I tend to forget and look for afterward

## Archives

Extract compressed archives with correct options

```bash
tar -xjf  comp-archive-123.tar.bz2 # -j (bzip2)
tar -xvzf comp-archive-123.tar.gz  # -z (gzip)
tar -xf   comp-archive-123.tar.xz
```

## Assembly

### x86_64

Using *nasm* to assemble and GNU linker to generate executable

``` bash
nasm -f elf64 <prog>.asm -g # -f = format
ld <prog>.o -o <prog>
```

## Compilation

Saving intermediate files from compilation (.i, .s, .o) using *gcc*

```bash
gcc main.c --save-temps -o main
```

Compile and link program w/ static library

```bash
gcc main.c -o main -I libs -L libs -labcd
# .
# ├── libs
# │   ├── libabcd.a
# │   └── libabcd.h
# └── main.c
```

## Databases

sqlite3 usage

```bash
sqlite3 dbname.sqlite
.schema # display database schema
.exit
```

## Debugging

### GDB

Using GNU debugger to inspect code in *asm*

```bash
gcc prog.c -o prog -g # compile source using debugging options
gdb ./prog # start gdb
(gdb) b main # set breakpoint at main entry point
(gdb) run # start program
(gdb) layout asm # apply the asm layout for inspection
```

disassembly style

```bash
(gdb) show disassembly-flavor
(gdb) set disassembly-flavor intel
(gdb) set disassembly-flavor att
```

## Docker

### MySQL container

Connect to MySQL container

```bash
docker exec -it <container_name> mysql -u<user> -p<pass>
```

Export database

```bash
docker exec -i <container_name> mysqldump --no-tablespaces -u<user> -p<pass> <db_name> > <backup_name>.sql
```

## File manipulations

Rename and remove *underscore* from files in current folder

```bash
for f in _*; do mv -v "$f" $(echo "$f" | tr -d '_'); done
```

Generate *html* file from *RTF* files based on their basename and content

```bash
for f in *.rtf; do echo -E "$(basename -s .rtf "## $f") $(unrtf --quiet "$f")" >> index.html; done
```

Create the same file in all subdirectories

```bash
for d in ./*/; do cd "$d" && touch file.txt && cd ..; done
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

Find modified files at a certain date 

```bash
find -type f -name "*.cpp" -newermt yyyy-mm-dd
```

Copy output of command to clipboard

```bash
pwd | xclip -i 
```

Display file permission bits in octal

```bash
stat -c "%a %n" <filename>
```

Display lines (range) in file

```bash
sed -n 1,10p file.txt
```

Delete lines (range) in file

```bash
sed -i 1,10d file.txt # option -i : --in-place
```

## Git

Remove remote origin of git repository

```bash
git remote remove origin
```

Change remote origin of git repository

```bash
git remote add origin git@github.com:User/repo.git
```

Changing commit message that hasn't been pushed

```bash
git commit --amend -m "modified commit msg"
```

## ImageMagick

Resize image while keeping (height or width) ratio

```bash
mogrify -resize x450 my_img.jpg # preserve height
mogrify -resize 850x my_img.jpg # preserve width
```

Crop image

```bash
mogrify -crop 100x100+0+0 my_img.jpg # crop 100x100px from x=0, y=0
```

Convert format

```bash
mogrify -format jpeg image.ppm # from .ppm to .jpeg
```

Display image size infos

```bash
identify -format "%wx%h" img.jpg
```

Vertically stack images

```bash
convert <img-1> <img-2> -append <result-img>
```

> +append : to concatenate images horizontally

## Integrity check

Compute SHA256 message digest to verify file integrity

```bash
echo "<filehash> <file>" | sha256sum --c
```

## Network

### iproute2

Display current available network interfaces

```bash
ip -c -br link
```

Display IPv4 addresses

```bash
ip -c -br -4 a
```

## OpenSSL

Generate new private key

```bash
openssl genpkey -out <pri_key> \ 
-algorithm RSA \
-pkeyopt rsa_keygen_bits:<key-size> \
-aes-256-cbc
```

> key is protected with passphrase using AES-256

Derive public key

```bash
openssl rsa -in <pri_key> -pubout -out <pub_key>
```

Key structure information

```bash
openssl rsa -in <pri_key> -text -noout
```

> add `-pubin` params to specify public key

## Python

Setup environment and install project dependencies w/ pip

```bash
python3 -m venv venv # will create target dir 'venv' for virtual env
source venv/bin/activate
pip install -r requirements.txt
```

Save current state of virtual environment in `requirements.txt` : 

```bash
pip freeze >> requirements.txt
```

## Resolution

Display current screen resolution

```bash
xdpyinfo | grep "dimensions"
```

## Scripts

### Adding custom commands (from bash scripts)

Add new script inside `/home/user/` dedicated directory

```bash 
├── scripts
└────── custom.sh
```

add execution / read permissions

```bash
chmod u+rx custom.sh
```

create symlink from `.local/bin` folder to make script available from the command line : 

```bash
ln -s /home/user/scripts/custom.sh custom.sh
```

declare function inside `/home/user/.bash_functions`: 

```bash
function call_script {
	~/.local/bin/custom.sh
}
```

then source : 

```bash
source .bash_functions
```

## SSH pubkey

copy (*rsa*) public key content to clipboard

```bash
xclip -sel clip < ~/.ssh/id_rsa.pub # shorthand for : -select clipboard
```

## Snap

Updating snap store

```bash
snap-store --quit && sudo snap refresh snap-store
```

## Tmux

### Basic usage

 `ctrl` + `b`  : default prefix

**Panel split**

`prefix` + `%` : vertical split 

`prefix` + `"` : horizontal split

`prefix` + `q` : panel number

`prefix` + `o` : switch between panels

`prefix`+ `alt` + `↑ ↓ ← →` : to increase / decrease panel size 

`prefix` + `z` : zoom / de-zoom

`prefix` + `x` : close panel

### Plugins

to install new plugins, edit `.tmux.conf` :

```bash
set -g @plugin 'plugin/path'
```

then `prefix` + `I`  to fetch the plugin

## Vim

search and replace pattern (all occurences)

```bash
:%s/{find}/{replace}/g
```

## Wget

Saving remote file in specified directory

```bash
wget https://.../resource.jpg -P /to/directory/path
```

