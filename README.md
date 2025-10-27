# 🐧 Linux Essentials Reference

## 📘 Getting Help in Linux

### MAN Pages

```bash
man command        # Example: man ls
```

* **Displayed using:** `less`
* **Shortcuts:**

  * `h` → Help
  * `q` → Quit
  * `Enter` → Next line
  * `Space` → Next screen
  * `/string` → Search forward
  * `?string` → Search backward
  * `n / N` → Next / Previous match

---

### Check Command Type

```bash
type rm      # rm is /usr/bin/rm
type cd      # cd is a shell builtin
```

---

### Get Help for Built-in Commands

```bash
help command     # Example: help cd
command --help   # Example: rm --help
```

---

### Search for Commands or Keywords

```bash
man -k uname
man -k "copy files"
apropos passwd
```

---

## 🧠 Bash History

### Viewing and Managing History

```bash
history           # Show command history
history -d 100    # Delete entry number 100
history -c        # Clear entire history
```

### History Configuration

```bash
echo $HISTFILESIZE   # Commands saved in history file (~/.bash_history)
echo $HISTSIZE        # Commands saved in memory
```

### Re-executing Commands

```bash
!!          # Rerun last command
!20         # Run 20th command in history
!-10        # Run last 10th command
!abc        # Run last command starting with "abc"
!abc:p      # Print (not run) last command starting with "abc"
```

### Searching and Timestamping

* **Reverse search:** `CTRL + R`
* **Add timestamps to history:**

  ```bash
  HISTTIMEFORMAT="%d/%m/%y %T"
  echo 'HISTTIMEFORMAT="%d/%m/%y %T"' >> ~/.bashrc
  ```

---

## 🔑 Running Commands as Root

### Using `sudo` and `su`

```bash
sudo command       # Run command as root
sudo su            # Become root temporarily (user password)
sudo passwd root   # Set root password
passwd username    # Change user password
su                 # Become root (root password)
```

---

## 📂 Linux Paths

| Symbol | Meaning               |
| ------ | --------------------- |
| `.`    | Current directory     |
| `..`   | Parent directory      |
| `~`    | User’s home directory |

### Common Commands

```bash
cd                 # Go to home directory
cd ~               # Go to home directory
cd -               # Return to previous directory
cd /path_to_dir    # Change to specified directory
pwd                # Print working directory
```

### Tree Command

```bash
sudo apt install tree
tree directory/    # Example: tree .
tree -d .          # Only directories
tree -f .          # Absolute paths
```

---

## 📜 The `ls` Command

### Syntax

```bash
ls [OPTIONS] [FILES]
```

### Examples

| Option   | Description                        | Example                    |
| -------- | ---------------------------------- | -------------------------- |
| *(none)* | List current directory             | `ls`                       |
| `-l`     | Long listing format                | `ls -l ~`                  |
| `-a`     | Include hidden files               | `ls -la ~`                 |
| `-1`     | Single column output               | `ls -1 /etc`               |
| `-d`     | Show directory info (not contents) | `ls -ld /etc`              |
| `-h`     | Human-readable sizes               | `ls -h /etc`               |
| `-S`     | Sort by size                       | `ls -Sh /var/log`          |
| `-X`     | Sort by extension                  | `ls -lX /etc`              |
| `--hide` | Hide matching files                | `ls --hide=*.log /var/log` |
| `-R`     | Recursive listing                  | `ls -lR ~`                 |
| `-i`     | Show inode numbers                 | `ls -li /etc`              |

> 🛈 Note: `ls` does **not** show the total size of directories. Use `du` instead:
>
> ```bash
> du -sh ~
> ```

---

## 🕒 File Timestamps and Date

### Viewing File Times

| Command                   | Description               |
| ------------------------- | ------------------------- |
| `ls -lu`                  | Access time (atime)       |
| `ls -l` / `ls -lt`        | Modification time (mtime) |
| `ls -lc`                  | Change time (ctime)       |
| `stat file.txt`           | All timestamps            |
| `ls -l --full-time /etc/` | Full timestamp display    |

### Modifying Timestamps

```bash
touch file.txt                # Create or update file
touch -a file                 # Update access time
touch -m file                 # Update modification time
touch -m -t 201812301530.45 a.txt  # Set specific time
touch -d "2010-10-31 15:45:30" a.txt  # Custom date/time
touch a.txt -r b.txt          # Match timestamps with another file
```

### Date and Calendar Commands

```bash
date                          # Show date and time
date --set="2 OCT 2020 18:00:00"  # Set date/time
cal                           # Current month
cal 2021                      # Year view
cal 7 2021                    # July 2021
cal -3                        # Previous, current, next months
```

### Sorting by Time

```bash
ls -l                 # Sort by name
ls -lt                # Sort by modification time
ls -ltu               # Sort by access time
ls -ltu --reverse     # Reverse order
```

---

## 📄 Viewing Files

### `cat`, `less`, `more`, `head`, `tail`, `watch`

#### Displaying Files

```bash
cat filename
cat filename1 filename2
cat filename1 filename2 > filename3    # Concatenate files
```

> 🛈 To show line numbers:
>
> ```bash
> cat -n filename
> ```

#### Viewing with `less`

```bash
less filename
```

**Shortcuts:**

* `h` → Help
* `q` → Quit
* `Enter` → Next line
* `Space` → Next screen
* `/string` / `?string` → Search forward/backward
* `n / N` → Next / Previous match

#### Head & Tail

```bash
head filename           # First 10 lines
head -n 15 filename     # First 15 lines
tail filename           # Last 10 lines
tail -n 15 filename     # Last 15 lines
tail -n +5 filename     # From line 5 onward
tail -f filename        # Real-time updates
```

#### Watch Command

```bash
watch -n 3 ls -l        # Refresh every 3 seconds
```

---

## 📁 Working with Files and Directories (touch, mkdir, cp, mv, rm, shred)

### Creating and Managing Files/Directories

```bash
# creating a new file or updating the timestamps if the file already exists
touch filename

# creating a new directory
mkdir dir1

# creating a directory and its parents as well
mkdir -p mydir1/mydir2/mydir3
```

---

### The `cp` command

```bash
# copying file1 to file2 in the current directory
cp file1 file2

# copying file1 to dir1 as another name (file2)
cp file1 dir1/file2

# copying a file prompting the user if it overwrites the destination
cp -i file1 file2

# preserving the file permissions, group and ownership when copying
cp -p file1 file2

# being verbose
cp -v file1 file2

# recursively copying dir1 to dir2 in the current directory
cp -r dir1 dir2/

# copy more source files and directories to a destination directory
cp -r file1 file2 dir1 dir2 destination_directory/
```

---

### The `mv` command

```bash
# renaming file1 to file2
mv file1 file2

# moving file1 to dir1 
mv file1 dir1/

# moving a file prompting the user if it overwrites the destination file
mv -i file1 dir1/

# preventing an existing file from being overwritten
mv -n file1 dir1/

# moving only if the source file is newer than the destination file or when the destination file is missing
mv -u file1 dir1/

# moving file1 to dir1 as file2
mv file1 dir1/file2

# moving more source files and directories to a destination directory
mv file1 file2 dir1/ dir2/ destination_directory/
```

---

### The `rm` command

```bash
# removing a file
rm file1

# being verbose when removing a file
rm -v file1

# removing a directory
rm -r dir1/

# removing a directory without prompting
rm -rf dir1/

# removing a file and a directory prompting the user for confirmation
rm -ri file1 dir1/

# secure removal of a file (verbose with 100 rounds of overwriting)
shred -vu -n 100 file1
```

