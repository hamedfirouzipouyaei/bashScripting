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

## 🏗️ The Linux File System

A **file system** controls how data is stored and retrieved. Each group of data is called a **file**, and the structure and logic used to manage files and their names are called **file systems**.

* A file system is a logical collection of files on a partition or disk.  
* On a Linux system, *everything is considered to be a file*.  
* Linux file and directory names are **case-sensitive**.

---

## 📁 The Filesystem Hierarchy Standard (FHS)

The **Filesystem Hierarchy Standard (FHS)** defines the directory structure and directory contents in Linux distributions.

### Common Directories

* **/bin** — Contains binaries or user executable files available to all users.  
* **/sbin** — Contains applications for the superuser (system administration commands).  
* **/boot** — Holds files required for starting (booting) the system.  
* **/home** — User home directories. Each user has a subdirectory here (e.g. `/home/user1`).  
  * The root user has a separate home directory: `/root`.  
* **/dev** — Contains device files representing hardware devices.  
* **/etc** — Contains system-wide configuration files.  
* **/lib** — Shared library files used by different applications.  
* **/media** — Mount point for removable media (e.g., USB drives).  
* **/mnt** — Temporary mount point (less used today).  
* **/tmp** — Stores temporary files created by applications and users.  
* **/proc** — Virtual directory containing runtime system information (e.g., CPU, RAM, kernel).  
* **/sys** — Contains information about devices, drivers, and kernel features.  
* **/srv** — Contains data served by system services.  
* **/run** — Temporary filesystem that resides in RAM and stores runtime data.  
* **/usr** — Contains user binaries, libraries, documentation, and other utilities.  
  * Example: many distributions store executables in `/usr/bin` and `/usr/sbin`.  
* **/var** — Contains variable data like logs, caches, and spool files.  

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

## � Piping and Command Redirection

Every Linux command or program we run has **three data streams**:

1. **STDIN (0)** — Standard Input  
2. **STDOUT (1)** — Standard Output  
3. **STDERR (2)** — Standard Error  

### Using Pipes

A **pipe** (`|`) connects the output of one command to the input of another.

```bash
command1 | command2 | command3
```

This allows you to chain multiple commands together efficiently.

---

## �📁 Working with Files and Directories (`touch`, `mkdir`, `cp`, `mv`, `rm`, `shred`)

### Creating and Managing Files/Directories

```bash
# Create a new file or update its timestamps
touch filename

# Create a new directory
mkdir dir1

# Create a directory along with its parent directories
mkdir -p mydir1/mydir2/mydir3
```

---

### The `cp` Command — Copy Files and Directories

```bash
# Copy file1 to file2 in the current directory
cp file1 file2

# Copy file1 to dir1 as file2
cp file1 dir1/file2

# Copy a file, prompting before overwrite
cp -i file1 file2

# Preserve file permissions, group, and ownership
cp -p file1 file2

# Be verbose (show copied files)
cp -v file1 file2

# Recursively copy dir1 to dir2
cp -r dir1 dir2/

# Copy multiple files and directories to a destination
cp -r file1 file2 dir1 dir2 destination_directory/
```

---

### The `mv` Command — Move or Rename Files and Directories

```bash
# Rename file1 to file2
mv file1 file2

# Move file1 to dir1
mv file1 dir1/

# Prompt before overwriting a destination file
mv -i file1 dir1/

# Prevent overwriting an existing file
mv -n file1 dir1/

# Move only if the source is newer or the destination is missing
mv -u file1 dir1/

# Move file1 to dir1 and rename it file2
mv file1 dir1/file2

# Move multiple sources to a destination directory
mv file1 file2 dir1/ dir2/ destination_directory/
```

---

### The `rm` Command — Remove Files and Directories

```bash
# Remove a file
rm file1

# Verbosely remove a file
rm -v file1

# Remove a directory
rm -r dir1/

# Remove without prompting (force)
rm -rf dir1/

# Prompt before removing each file or directory
rm -ri file1 dir1/

# Securely remove a file (overwrite 100 times)
shred -vu -n 100 file1
```
