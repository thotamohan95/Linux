# File management in Linux

## Table of contents
- File & directory management
- Viewing and editing files
- vi (Vim) editor — quick reference for beginners
- File permissions and ownership
- Special permissions and umask
- Quick reference / examples

---

## File & directory management (basic commands)
- `ls` — list files and directories in the current directory.
- `cd /path/to/directory` — change working directory.
- `pwd` — print current working directory.
- `mkdir new_folder` — create a new directory.
- `rmdir empty_folder` — remove an empty directory.
- `rm file.txt` — delete a file (use carefully).
- `rm -r folder` — delete a directory and its contents recursively (DANGEROUS).
- `cp file1.txt file2.txt` — copy a file.
- `cp -r dir1 dir2` — copy a directory recursively.
- `mv old_name new_name` — move or rename a file or directory.

Tip for beginners: always double-check `rm -r` and consider `rm -i` to prompt before each removal.

---

## Viewing and editing files
- `cat file.txt` — display the whole file.
- `tac file.txt` — display file lines in reverse order.
- `less file.txt` — view a file with paging and navigation.
- `more file.txt` — simpler pager (forward only).
- `head -n 10 file.txt` — show first 10 lines.
- `tail -n 10 file.txt` — show last 10 lines.
- `nano file.txt` — simple, beginner-friendly text editor.
- `vi file.txt` — powerful editor (see vi/Vim section).
- `echo 'Hello' > file.txt` — overwrite file with "Hello".
- `echo 'Hello' >> file.txt` — append "Hello" to the file.

---

## vi (Vim) editor — quick reference for beginners

vi has three main modes:
- Normal mode (default) — navigation and commands.
- Insert mode — editing text (enter with `i`, exit with `Esc`).
- Command mode — for save/quit and complex commands (enter by pressing `:` in Normal mode).

Basic navigation (Normal mode)
- `h` — left
- `j` — down
- `k` — up
- `l` — right
- `0` — beginning of line
- `^` — first non-blank char on line
- `$` — end of line
- `w` — move to start of next word
- `b` — move to start of previous word
- `gg` — go to start of file
- `G` — go to end of file
- `:n` — go to line number `n` (e.g., `:10`)

Insert mode shortcuts
- `i` — insert before cursor
- `I` — insert at start of line
- `a` — append after cursor
- `A` — append at end of line
- `o` — open a new line below and enter Insert mode
- `O` — open a new line above and enter Insert mode
- `Esc` — return to Normal mode

Editing and clipboard
- `x` — delete character under cursor
- `X` — delete character before cursor
- `dw` — delete a word
- `dd` — delete a whole line
- `d$` or `D` — delete to end of line
- `d0` — delete to start of line
- `u` — undo
- `Ctrl + r` — redo
- `yy` — yank (copy) a line
- `yw` — yank (copy) a word
- `p` — paste after cursor
- `P` — paste before cursor

Search and replace
- `/pattern` — search forward
- `?pattern` — search backward
- `n` — repeat last search forward
- `N` — repeat last search backward
- `:%s/old/new/g` — replace all occurrences in file
- `:s/old/new/g` — replace in current line

Working with multiple files / windows
- `:e filename` — open another file
- `:w` — save
- `:wq` — save and quit
- `:q!` — quit without saving
- `:split filename` — horizontal split
- `:vsplit filename` — vertical split
- `Ctrl + w + w` — switch between splits

Tip: If new to vi/vim, try `nano` first. Use `vimtutor` to practice vi basics.

---

## File permissions and ownership (beginner-friendly)

Linux permissions control read, write, execute access. There are three classes:
- Owner (user) — the file owner.
- Group — users in the file's group.
- Others — everyone else.

Permission types and numeric values:
- Read (`r`) = 4
- Write (`w`) = 2
- Execute (`x`) = 1

To see permissions:
- `ls -l filename`

Example output:

```
-rwxr--r-- 1 user group 1234 Mar 28 10:00 myfile.sh
```
Breakdown:
- First character: `-` file, `d` directory, `l` symlink, etc.
- Next 9 chars: `rwx r-- r--` → owner, group, others.
- `rwx` for owner means read+write+execute (4+2+1 = 7).
- So `-rwxr--r--` corresponds to numeric `744`.

Using `chmod` (change mode)

Symbolic mode examples:
- `chmod u+x filename` — add execute for owner.
- `chmod g-w filename` — remove write for group.
- `chmod o=r filename` — set others to read-only.
- `chmod u=rwx,g=rx,o= filename` — owner=rwx, group=rx, others=none.

Numeric (octal) mode examples:
- `chmod 755 filename` — owner=rwx (7), group=rx (5), others=rx (5).
- `chmod 644 filename` — owner=rw (6), group=r (4), others=r (4).
- `chmod 700 filename` — owner=rwx (7), no access for group/others (0).

Quick conversion tip:
- Add the bits for each class: r(4)+w(2)+x(1). Example: r+w = 4+2 = 6.

Changing ownership
- `chown newuser filename` — change owner.
- `chown newuser:newgroup filename` — change owner and group.
- `chown :newgroup filename` — change only group.
- Recursively: `chown -R newuser:newgroup directory/`

Change group only
- `chgrp newgroup filename`
- Recursive: `chgrp -R newgroup directory/`

---

## Special permissions

- SetUID (user's execute bit shows `s`): run an executable as the file owner's permissions.
  - `chmod u+s filename`
  - Common example: `/usr/bin/passwd`

- SetGID (group execute bit shows `s`):
  - On files: run with file group permissions.
  - On directories: new files inherit the directory's group.
  - `chmod g+s directory/`

- Sticky bit (others execute bit shows `t`):
  - On directories: only the file owner (or root) can remove files.
  - `chmod +t directory/`
  - Example: `/tmp` typically has the sticky bit.

---

## umask — default new-file permissions
`umask` sets bits to remove from the default permissions for new files and directories.

- View current: `umask`
- Example: `umask 022` means:
  - New directories default to `rwxr-xr-x` (755).
  - New files default to `rw-r--r--` (644).

Note: Files are typically created without execute bit by default; directories need execute to be enterable.

---

## Quick reference / examples
- Make directory, move into it:
  ```
  mkdir project
  cd project
  ```
- Create a file and edit with nano:
  ```
  echo 'hello' > hello.txt
  nano hello.txt
  ```
- Make a script executable and run:
  ```
  chmod 755 script.sh
  ./script.sh
  ```
- Recursively set ownership to user `alice` and group `developers`:
  ```
  sudo chown -R alice:developers /path/to/dir
  ```

---

## Final tips for beginners
- Use `man <command>` (e.g., `man ls`) to read the manual for any command.
- Use `--help` (e.g., `ls --help`) for quick options.
- Be careful with `rm -r` and ownership changes — consider a backup or test in a safe directory.
- Try `vimtutor` if you want to learn `vi`/`vim` step-by-step.
