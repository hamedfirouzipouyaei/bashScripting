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
| `/`    | Root directory        |

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

## 🔄 Piping and Command Redirection

Every Linux command or program we run has **three data streams**:

1. **STDIN (0)** — Standard Input  
2. **STDOUT (1)** — Standard Output  
3. **STDERR (2)** — Standard Error  

`>` is the redirect operator.

```bash
# The following means:
# List all files (including hidden ones) in ../../my_folder,
# combine normal output and error output,
# and then show only the first 5 lines of that combined result.
ls -la ../../my_folder 2>&1 | head -5
```

### Using Pipes

A **pipe** (`|`) connects the output of one command to the input of another.

```bash
command1 | command2 | command3
```

This allows you to chain multiple commands together efficiently.

---

## 🔍 The `grep` Command — Pattern Searching

`grep` (Global Regular Expression Print) is one of the most powerful and frequently used commands in Linux for searching text patterns.

### `grep` Syntax

```bash
grep [OPTIONS] PATTERN [FILE...]
```

### Essential `grep` Options

| Option | Description                           | Example                        |
|--------|---------------------------------------|--------------------------------|
| `-i`   | Case-insensitive search              | `grep -i "error" logfile.txt`  |
| `-v`   | Invert match (exclude lines)         | `grep -v "debug" logfile.txt`  |
| `-n`   | Show line numbers                    | `grep -n "function" script.sh` |
| `-c`   | Count matching lines                 | `grep -c "error" logfile.txt`  |
| `-l`   | Show only filenames with matches     | `grep -l "TODO" *.txt`         |
| `-r`   | Recursive search in directories      | `grep -r "password" /etc/`     |
| `-w`   | Match whole words only               | `grep -w "cat" file.txt`       |
| `-A n` | Show n lines after match             | `grep -A 3 "error" log.txt`    |
| `-B n` | Show n lines before match            | `grep -B 2 "error" log.txt`    |
| `-C n` | Show n lines before and after match  | `grep -C 2 "error" log.txt`    |

---

## 🚰 Powerful Pipe Examples

### Basic Pipe Operations

```bash
# File listing and filtering
ls -lSh /etc/ | head                # See the first 10 files by size
ls -la | grep "\.txt$"              # Find files ending with .txt
ls -la | grep "^d" | wc -l          # Count directories

# Process management
ps -ef | grep sshd                  # Check if sshd is running
ps aux | grep firefox               # Search for specific processes
ps aux --sort=-%mem | head -n 3     # Show top 3 processes by memory consumption

# Search command history
history | grep "git"

# Search in multiple files
cat *.log | grep "ERROR"
```

### Advanced Pipe Combinations

```bash
# Find largest files in current directory
ls -la | grep "^-" | sort -k5 -n | tail -5

# Search for processes and kill them
ps aux | grep "python" | grep -v grep | awk '{print $2}' | xargs kill

# Find files modified today and search for patterns
find . -type f -mtime 0 | xargs grep -l "function"

# Monitor log files in real-time
tail -f /var/log/syslog | grep --color=always "error"

# Search and format output
grep -r "TODO" . | cut -d: -f1 | sort | uniq

# Chain multiple filters
cat access.log | grep "404" | grep -v "bot" | awk '{print $1}' | sort | uniq -c | sort -nr
```

### File Content Analysis

```bash
# Find duplicate lines
sort file.txt | uniq -d

# Count word frequency
cat file.txt | tr ' ' '\n' | sort | uniq -c | sort -nr

# Extract email addresses
grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' contacts.txt

# Find IP addresses in logs
grep -oE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' access.log
```

### System Administration Examples

```bash
# Check disk usage of directories
du -sh */ | sort -hr | head -10

# Find large files
find / -type f -size +100M 2>/dev/null | head -10

# Check network connections
netstat -an | grep ESTABLISHED | wc -l

# Monitor CPU usage
ps aux | sort -k3 -nr | head -10

# Search configuration files
find /etc -name "*.conf" | xargs grep -l "database"
```

### Security and Log Analysis

```bash
# Monitor authentication failures
cat -n /var/log/auth.log | grep -ai "authentication failure" | wc -l

# Save authentication failures to file (combining piping and redirection)
cat -n /var/log/auth.log | grep -ai "authentication failure" > auth.txt

# Analyze failed login attempts with details
grep "Failed password" /var/log/auth.log | awk '{print $1, $2, $3, $11}' | sort | uniq -c

# Monitor real-time authentication events
tail -f /var/log/auth.log | grep --color=always -i "failed\|success\|invalid"
```

### Redirection Operators

```bash
# Basic output redirection
ps aux > running_processes.txt         # Save process list to file
who -H > loggedin_users.txt            # Save logged-in users to file
command > file.txt                     # Overwrite file
command >> file.txt                    # Append to file

# Practical redirection examples  
id >> loggedin_users.txt               # Append user ID info to existing file

# Separate output and error streams
tail -n 10 /var/log/*.log > output.txt 2> errors.txt    # Separate files
command > output.txt 2> error.txt                       # Standard separation

# Redirect both stdout and stderr to same file
tail -n 2 /etc/passwd /etc/shadow > output_errors.txt 2>&1    # Combined output
command > file.txt 2>&1                                       # Redirect both to file

# Discard unwanted output
command 2>/dev/null                    # Discard errors only
command > /dev/null 2>&1               # Discard all output

# Input redirection
grep "pattern" < input.txt

# Here document
grep "search" << EOF
line 1
line 2 with search term
line 3
EOF
```

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
# Securely remove a file (overwrite 100 times)
shred -vu -n 100 file1
```

---

## 🔍 Finding Files (`find`, `locate`)

Linux provides powerful tools for locating files and directories across the filesystem.

---

## 📍 The `locate` Command (plocate)

`locate` is a fast way to find files by name using a pre-built database. It's typically a symlink to `plocate` on modern systems.

### Database Management

```bash
# Update the locate database (run as root)
sudo updatedb

# The database is usually updated daily via cron
```

### Basic `locate` Usage

```bash
# Find files by name (expands to *filename*)
locate filename

# Case-insensitive search
locate -i filename

# Find by exact name using regex
locate -r '/filename$'

# Search only the basename (not full path)
locate -b filename

# Use regular expressions
locate -r 'regex'

# Check that located files actually exist
locate -e filename

# Limit number of results
locate -l 10 filename
```

### Practical `locate` Examples

```bash
# Find all Python files
locate "*.py"

# Find configuration files
locate -i config

# Find files in specific directory
locate -r '^/etc/.*\.conf$'

# Find recently accessed files (combine with other tools)
locate "*.log" | head -20
```

---

## 🔎 The `which` Command

Find the location of executable commands in your PATH.

```bash
# Show command path
which command

# Show all matching paths
which -a command

# Examples
which python3           # /usr/bin/python3
which -a python         # Show all python binaries in PATH
which ls                # /usr/bin/ls
which cd                # (no output - cd is a shell builtin)
```

---

## 🔍 The `find` Command

`find` is the most powerful and flexible file searching tool in Linux.

### `find` Syntax

```bash
find PATH [OPTIONS] [ACTIONS]
```

### Essential `find` Options

| Option | Description | Example |
|--------|-------------|---------|
| `-type f` | Regular files | `find /home -type f` |
| `-type d` | Directories | `find /var -type d` |
| `-type l` | Symbolic links | `find /usr -type l` |
| `-name pattern` | Match filename (case-sensitive) | `find . -name "*.txt"` |
| `-iname pattern` | Match filename (case-insensitive) | `find . -iname "*.TXT"` |
| `-size +n` | Files larger than n | `find . -size +1M` |
| `-size -n` | Files smaller than n | `find . -size -1K` |
| `-perm mode` | Files with specific permissions | `find . -perm 755` |
| `-user owner` | Files owned by user | `find /home -user john` |
| `-group group` | Files owned by group | `find /var -group www-data` |
| `-mtime n` | Modified n days ago | `find . -mtime -7` |
| `-atime n` | Accessed n days ago | `find . -atime +30` |
| `-ctime n` | Changed n days ago | `find . -ctime -1` |

### Practical `find` Examples

```bash
# Find large files (bigger than 1MB)
find ~ -type f -size +1M

# Find empty files and directories
find /tmp -empty

# Find files modified in last 7 days
find /var/log -type f -mtime -7

# Find files with specific permissions
find /usr/bin -type f -perm 755

# Find and list files with details
find /etc -name "*.conf" -ls

# Find files by multiple criteria
find /home -type f -name "*.jpg" -size +1M -mtime -30

# Find and execute commands
find /tmp -name "*.tmp" -delete
find /var/log -name "*.log" -exec ls -lh {} \;

# Find files excluding certain directories
find / -type f -name "*.log" -not -path "/proc/*" -not -path "/sys/*"

# Find by inode number
find /home -inum 123456

# Find files with multiple links
find /usr -type f -links +1

# Combine with other commands
find /var/log -name "*.log" | xargs grep -l "error"
find /home -type f -name "*.txt" | xargs wc -l
```

### Advanced `find` Usage

```bash
# Find files and copy them
find /source -name "*.backup" -exec cp {} /destination/ \;

# Find and archive files
find /logs -name "*.log" -mtime +30 -exec tar -czf old_logs.tar.gz {} +

# Find files by content and size
find /var -type f -size +10M -exec grep -l "error" {} \;

# Find files with complex time conditions
find /tmp -type f \( -atime +7 -o -mtime +7 \)

# Find setuid files (security check)
find /usr -type f -perm -4000 -ls

# Find world-writable files
find /home -type f -perm -002

# Find files newer than a reference file
find /etc -newer /etc/passwd
```

---

## 🔬 Comparing Files and Checksums

Linux provides powerful tools for comparing files and verifying file integrity using checksums.

---

## 📊 The `cmp` Command - Binary Comparison

`cmp` compares two files byte by byte. It's useful for binary files and quick comparisons.

### `cmp` Syntax

```bash
cmp [OPTIONS] file1 file2
```

### Basic `cmp` Usage

```bash
# Compare two files
cmp file1.txt file2.txt

# Silent mode (only exit status)
cmp -s file1.txt file2.txt
echo $?    # 0 = identical, 1 = different, 2 = error

# Show all differences
cmp -l file1.txt file2.txt

# Print differing bytes
cmp -b file1.txt file2.txt
```

### Practical `cmp` Examples

```bash
# Check if two files are identical
if cmp -s file1 file2; then
    echo "Files are identical"
else
    echo "Files differ"
fi

# Compare binary files
cmp image1.jpg image2.jpg

# Find first difference
cmp file1 file2

# Output example: file1 file2 differ: byte 45, line 3
```

### `cmp` vs `diff`

* **`cmp`**: Byte-by-byte comparison, stops at first difference, good for binary files
* **`diff`**: Line-by-line comparison, shows all differences, best for text files

---

## 🔀 The `diff` Command - Text Comparison

`diff` compares files line by line and shows the differences. Essential for code review and version control.

### `diff` Syntax

```bash
diff [OPTIONS] file1 file2
```

### Basic `diff` Usage

```bash
# Basic comparison
diff file1.txt file2.txt

# Unified format (most readable)
diff -u file1.txt file2.txt

# Context format (shows surrounding lines)
diff -c file1.txt file2.txt

# Side-by-side comparison
diff -y file1.txt file2.txt

# Brief output (only report if files differ)
diff -q file1.txt file2.txt
```

### `diff` Options

| Option | Description | Example |
|--------|-------------|---------|
| `-u` | Unified format (Git-style) | `diff -u old.txt new.txt` |
| `-c` | Context format | `diff -c old.txt new.txt` |
| `-y` | Side-by-side | `diff -y old.txt new.txt` |
| `-q` | Brief (files differ or not) | `diff -q file1 file2` |
| `-i` | Ignore case | `diff -i file1 file2` |
| `-w` | Ignore whitespace | `diff -w file1 file2` |
| `-b` | Ignore whitespace changes | `diff -b file1 file2` |
| `-B` | Ignore blank lines | `diff -B file1 file2` |
| `-r` | Recursive (directories) | `diff -r dir1 dir2` |
| `-N` | Treat absent files as empty | `diff -rN dir1 dir2` |

### Understanding `diff` Output

#### Normal Format

```bash
diff file1.txt file2.txt
# Output:
# 3c3
# < This is line 3 in file1
# ---
# > This is line 3 in file2

# Explanation:
# 3c3    = line 3 changed
# <      = from file1
# >      = from file2
# a      = added
# d      = deleted
# c      = changed
```

#### Unified Format (Most Common)

```bash
diff -u file1.txt file2.txt
# Output:
# --- file1.txt
# +++ file2.txt
# @@ -1,5 +1,5 @@
#  Line 1
#  Line 2
# -Old line 3
# +New line 3
#  Line 4

# Explanation:
# ---    = original file
# +++    = modified file
# @@     = line range
# -      = removed line
# +      = added line
# (space) = unchanged line
```

### Practical `diff` Examples

```bash
# Compare configuration files
diff -u /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup

# Compare directories recursively
diff -r directory1 directory2

# Ignore whitespace differences
diff -w original.txt modified.txt

# Create a patch file
diff -u original.c modified.c > changes.patch

# Apply a patch
patch original.c < changes.patch

# Compare and show only files that differ
diff -qr dir1 dir2

# Side-by-side with width
diff -y -W 150 file1.txt file2.txt

# Ignore case differences
diff -i file1.txt file2.txt

# Compare and highlight differences in color
diff --color file1.txt file2.txt
```

### Comparing Directories

```bash
# Basic directory comparison
diff -r dir1 dir2

# Show only which files differ
diff -qr dir1 dir2

# Exclude certain files
diff -r --exclude="*.log" dir1 dir2

# Exclude multiple patterns
diff -r --exclude="*.log" --exclude="*.tmp" dir1 dir2

# Compare with new files treated as empty
diff -rN dir1 dir2
```

---

## 🔐 The `sha256sum` Command - File Integrity

`sha256sum` generates and verifies SHA-256 cryptographic checksums. Essential for verifying file integrity and detecting changes.

### What is SHA-256?

* **SHA-256**: Secure Hash Algorithm producing a 256-bit (64 character hex) hash
* **Use cases**: Verify downloads, detect file tampering, ensure data integrity
* **Property**: Even a tiny change produces a completely different hash

### `sha256sum` Syntax

```bash
sha256sum [OPTIONS] file
```

### Basic `sha256sum` Usage

```bash
# Generate checksum for a file
sha256sum file.txt

# Generate checksums for multiple files
sha256sum file1.txt file2.txt file3.txt

# Generate and save checksums to a file
sha256sum *.txt > checksums.sha256

# Verify checksums from a file
sha256sum -c checksums.sha256

# Check specific file
echo "hash_value  filename" | sha256sum -c
```

### Practical `sha256sum` Examples

```bash
# Verify downloaded file
sha256sum ubuntu-22.04.iso
# Compare output with official checksum from website

# Create checksum file for directory
sha256sum /backup/*.tar.gz > backup-checksums.txt

# Verify all files
sha256sum -c backup-checksums.txt

# Verify only, suppress OK messages
sha256sum -c --quiet checksums.sha256

# Check and show only failures
sha256sum -c checksums.sha256 2>&1 | grep -v "OK"

# Generate checksum for data from pipe
echo "Hello World" | sha256sum

# Compare two files using checksums
if [ "$(sha256sum file1 | awk '{print $1}')" = "$(sha256sum file2 | awk '{print $1}')" ]; then
    echo "Files are identical"
else
    echo "Files differ"
fi
```

### Creating and Verifying Checksums

```bash
# Create checksums for all files in directory
find /data -type f -exec sha256sum {} \; > all-checksums.txt

# Verify downloads
# 1. Download file
wget https://example.com/software.tar.gz

# 2. Download official checksum
wget https://example.com/software.tar.gz.sha256

# 3. Verify
sha256sum -c software.tar.gz.sha256

# Create checksums recursively
find . -type f ! -name "checksums.txt" -exec sha256sum {} \; > checksums.txt

# Verify and show summary
sha256sum -c checksums.txt | tail -n 1
```

---

## 🔍 Other Checksum Commands

### `md5sum` - MD5 Checksums

```bash
# Generate MD5 checksum (128-bit)
md5sum file.txt

# Verify MD5 checksums
md5sum -c checksums.md5

# Note: MD5 is faster but less secure than SHA-256
```

### `sha1sum` - SHA-1 Checksums

```bash
# Generate SHA-1 checksum (160-bit)
sha1sum file.txt

# Verify SHA-1 checksums
sha1sum -c checksums.sha1

# Note: SHA-1 is deprecated for security purposes
```

### `sha512sum` - SHA-512 Checksums

```bash
# Generate SHA-512 checksum (512-bit, most secure)
sha512sum file.txt

# Verify SHA-512 checksums
sha512sum -c checksums.sha512

# Note: SHA-512 is more secure but slower than SHA-256
```

### Checksum Comparison

| Algorithm | Bits | Hex Length | Security | Speed | Use Case |
|-----------|------|------------|----------|-------|----------|
| MD5 | 128 | 32 | ⚠️ Weak | Fast | Legacy, non-security |
| SHA-1 | 160 | 40 | ⚠️ Deprecated | Fast | Legacy systems |
| SHA-256 | 256 | 64 | ✅ Strong | Moderate | Recommended |
| SHA-512 | 512 | 128 | ✅ Very Strong | Slower | High security |

---

## 🛠️ Practical Comparison Workflows

### Backup Verification

```bash
# Before backup
sha256sum /important/data/* > /backup/checksums-before.txt

# After backup
cd /backup/data
sha256sum * > checksums-after.txt

# Compare checksums
diff checksums-before.txt checksums-after.txt
```

### Configuration Change Detection

```bash
# Baseline
sha256sum /etc/*.conf > /var/baseline/config-checksums.txt

# Later, check for changes
sha256sum /etc/*.conf | diff - /var/baseline/config-checksums.txt
```

### Find Duplicate Files

```bash
# Find duplicates by checksum
find /media -type f -exec sha256sum {} \; | sort | uniq -w64 --all-repeated=separate
```

### Verify Directory Synchronization

```bash
# Generate checksums for source
cd /source
find . -type f -exec sha256sum {} \; | sort > /tmp/source-checksums.txt

# Generate checksums for destination
cd /destination
find . -type f -exec sha256sum {} \; | sort > /tmp/dest-checksums.txt

# Compare
diff /tmp/source-checksums.txt /tmp/dest-checksums.txt
```

---

## 📋 Quick Reference Summary

### When to Use Each Tool

* **`cmp`**: Quick binary comparison, check if files are identical
* **`diff`**: Compare text files, create patches, code review
* **`sha256sum`**: Verify downloads, detect tampering, ensure integrity

### Common Patterns

```bash
# Are two files identical?
cmp -s file1 file2 && echo "Same" || echo "Different"

# What changed between files?
diff -u original.txt modified.txt

# Verify file wasn't tampered with
sha256sum -c file.sha256

# Compare directories
diff -qr dir1 dir2

# Check file integrity after transfer
sha256sum original.iso > checksum.txt
# ... transfer file ...
sha256sum -c checksum.txt
```

---

## 🗜️ File Compression and Archiving

Linux provides powerful tools for compressing files and creating archives. The most common are `tar` (for archiving) and `gzip` (for compression).

### 📦 The `tar` Command - Tape Archive

`tar` creates archive files (combining multiple files into one) but doesn't compress by default. It's often used with compression tools.

**Common `tar` flags:**

| Flag | Meaning | Description |
|------|---------|-------------|
| `-c` | **c**reate | Create a new archive |
| `-x` | e**x**tract | Extract files from archive |
| `-t` | lis**t** | List contents of archive |
| `-v` | **v**erbose | Show files being processed |
| `-f` | **f**ile | Specify archive filename (must be last flag) |
| `-z` | g**z**ip | Compress/decompress with gzip |
| `-j` | b**j**ip2 | Compress/decompress with bzip2 |
| `-J` | xz | Compress/decompress with xz |
| `-C` | **C**hange directory | Extract to specific directory |

#### Basic `tar` Examples

```bash
# Create an archive
tar -cf archive.tar file1 file2 directory/

# Create archive with gzip compression
tar -czf archive.tar.gz file1 file2 directory/

# Create archive with verbose output
tar -cvzf archive.tar.gz directory/

# Extract archive
tar -xf archive.tar

# Extract gzipped archive
tar -xzf archive.tar.gz

# Extract to specific directory
tar -xzf archive.tar.gz -C /destination/path/

# List contents without extracting
tar -tzf archive.tar.gz

# Extract specific file
tar -xzf archive.tar.gz path/to/specific/file
```

#### Understanding `tar` Flag Combinations

```bash
# tar -czf archive.tar.gz files/
# c = create archive
# z = compress with gzip
# f = use this filename (archive.tar.gz)

# tar -xzvf archive.tar.gz
# x = extract
# z = decompress with gzip
# v = verbose (show files)
# f = from this filename
```

---

### 🗜️ The `gzip` Command - GNU Zip

`gzip` compresses individual files, replacing the original with a `.gz` version. For multiple files, use with `tar`.

**Common `gzip` flags:**

| Flag | Description |
|------|-------------|
| `-c` | Write to stdout (keep original) |
| `-d` | Decompress (same as `gunzip`) |
| `-k` | Keep original file |
| `-r` | Recursive (compress directories) |
| `-v` | Verbose (show compression ratio) |
| `-1` to `-9` | Compression level (1=fastest, 9=best) |

#### Basic `gzip` Examples

```bash
# Compress a file (replaces original)
gzip file.txt
# Result: file.txt.gz

# Compress and keep original
gzip -k file.txt

# Decompress
gzip -d file.txt.gz
# OR
gunzip file.txt.gz

# Compress with best compression
gzip -9 file.txt

# Compress and show details
gzip -v file.txt

# Compress multiple files
gzip file1.txt file2.txt file3.txt
```

---

### 🔗 Common Compression Workflows

#### Create Compressed Archives

```bash
# Compress directory with gzip (most common)
tar -czf backup.tar.gz /path/to/directory/

# Compress with bzip2 (better compression, slower)
tar -cjf backup.tar.bz2 /path/to/directory/

# Compress with xz (best compression, slowest)
tar -cJf backup.tar.xz /path/to/directory/
```

#### Extract Compressed Archives

```bash
# Extract .tar.gz
tar -xzf archive.tar.gz

# Extract .tar.bz2
tar -xjf archive.tar.bz2

# Extract .tar.xz
tar -xJf archive.tar.xz

# Extract to specific location
tar -xzf archive.tar.gz -C /destination/
```

#### Compression Comparison

| Format | Extension | Speed | Compression | Command |
|--------|-----------|-------|-------------|---------|
| gzip | `.tar.gz` or `.tgz` | Fast | Good | `tar -czf` |
| bzip2 | `.tar.bz2` | Medium | Better | `tar -cjf` |
| xz | `.tar.xz` | Slow | Best | `tar -cJf` |

**💡 Tip:** Use `.tar.gz` for general purposes - it's the standard and has good balance of speed and compression.

---

### 📋 Quick Reference

```bash
# Create compressed archive
tar -czf archive.tar.gz directory/

# Extract compressed archive
tar -xzf archive.tar.gz

# View contents
tar -tzf archive.tar.gz

# Compress single file
gzip file.txt

# Decompress single file
gunzip file.txt.gz
```

---

## 👥 User Account Management

Linux is a multi-user system where proper user account management is essential for security and system administration.

---

## 🔐 Password and Shadow Files

### `/etc/passwd` - User Account Information

The `/etc/passwd` file stores basic user account information. It's readable by all users but only writable by root.

**File format:** Each line represents a user with 7 fields separated by colons:

```text
username:x:UID:GID:comment:home_directory:shell
```

**Field breakdown:**

1. **Username**: Login name
2. **Password**: `x` means password is in `/etc/shadow`
3. **UID**: User ID (0 = root, 1-999 = system, 1000+ = regular users)
4. **GID**: Primary Group ID
5. **Comment**: Full name or description (GECOS field)
6. **Home Directory**: User's home directory path
7. **Shell**: Default shell for the user

**View passwd file:**

```bash
# View entire file
cat /etc/passwd

# View specific user
grep username /etc/passwd

# Example line:
# john:x:1001:1001:John Doe:/home/john:/bin/bash
```

### `/etc/shadow` - Encrypted Passwords

The `/etc/shadow` file stores encrypted passwords and password policies. Only readable by root for security.

**File format:** 9 fields separated by colons:

```text
username:encrypted_password:last_change:min:max:warn:inactive:expire:reserved
```

**Field breakdown:**

1. **Username**: Login name
2. **Encrypted Password**: Hashed password (or `!`/`*` if locked)
3. **Last Change**: Days since Jan 1, 1970 when password was last changed
4. **Minimum Days**: Days before password can be changed again
5. **Maximum Days**: Days until password must be changed
6. **Warning Days**: Days before expiration to warn user
7. **Inactive Days**: Days after expiration before account is disabled
8. **Expiration Date**: Date when account expires
9. **Reserved**: For future use

**View shadow file (requires root):**

```bash
# View entire file
sudo cat /etc/shadow

# View specific user
sudo grep username /etc/shadow
```

---

## 👤 Groups and User ID

### Understanding Groups

Groups allow multiple users to share access to files and resources.

**Group types:**

* **Primary Group**: User's main group (GID in `/etc/passwd`)
* **Secondary Groups**: Additional groups user belongs to

### `/etc/group` - Group Information

```bash
# View all groups
cat /etc/group

# Format: group_name:password:GID:user_list
# Example: developers:x:1005:john,alice,bob
```

### The `id` Command

Display user and group information:

```bash
# Show current user's info
id

# Show specific user's info
id username

# Show only UID
id -u username

# Show only GID
id -g username

# Show all groups
id -G username

# Show group names instead of numbers
id -Gn username

# Examples:
id
# Output: uid=1001(john) gid=1001(john) groups=1001(john),27(sudo),1005(developers)
```

### The `groups` Command

List groups a user belongs to:

```bash
# Show current user's groups
groups

# Show specific user's groups
groups username

# Example:
groups john
# Output: john : john sudo developers
```

---

## ➕ Creating Users

### The `useradd` Command

Create a new user account:

```bash
# Basic user creation
sudo useradd username

# Create user with home directory
sudo useradd -m username

# Create user with specific shell
sudo useradd -m -s /bin/bash username

# Create user with specific UID
sudo useradd -m -u 1500 username

# Create user with comment
sudo useradd -m -c "Full Name" username

# Create user and add to groups
sudo useradd -m -G sudo,developers username

# Full example with all options
sudo useradd -m -s /bin/bash -c "John Doe" -G sudo,developers john
```

**Common `useradd` options:**

| Option | Description | Example |
|--------|-------------|---------|
| `-m` | Create home directory | `useradd -m john` |
| `-d` | Specify home directory | `useradd -m -d /custom/path john` |
| `-s` | Specify shell | `useradd -s /bin/zsh john` |
| `-c` | Add comment/full name | `useradd -c "John Doe" john` |
| `-g` | Primary group | `useradd -g developers john` |
| `-G` | Secondary groups | `useradd -G sudo,docker john` |
| `-u` | Specify UID | `useradd -u 1500 john` |
| `-e` | Account expiration date | `useradd -e 2024-12-31 john` |

### Set User Password

```bash
# Set password for user
sudo passwd username

# You'll be prompted to enter password twice

# Force user to change password on first login
sudo passwd -e username
```

### The `adduser` Command (Debian/Ubuntu)

Interactive user creation (wrapper around `useradd`):

```bash
# Interactive user creation
sudo adduser username

# This will prompt you for:
# - Password
# - Full name
# - Room number
# - Work phone
# - Home phone
# - Other information
```

---

## ✏️ Modifying and Deleting Users

### The `usermod` Command - Modify User

Change user account attributes:

```bash
# Change username
sudo usermod -l newname oldname

# Change home directory
sudo usermod -d /new/home username

# Change default shell
sudo usermod -s /bin/zsh username

# Change user's comment
sudo usermod -c "New Full Name" username

# Add user to group (append)
sudo usermod -aG groupname username

# Change primary group
sudo usermod -g groupname username

# Lock user account
sudo usermod -L username

# Unlock user account
sudo usermod -U username

# Set account expiration
sudo usermod -e 2024-12-31 username

# Change UID
sudo usermod -u 1500 username
```

**Common `usermod` options:**

| Option | Description | Example |
|--------|-------------|---------|
| `-l` | Change username | `usermod -l newname old` |
| `-d` | Change home directory | `usermod -d /new/home user` |
| `-m` | Move home contents | `usermod -d /new/home -m user` |
| `-s` | Change shell | `usermod -s /bin/zsh user` |
| `-aG` | Add to groups (append) | `usermod -aG sudo user` |
| `-g` | Change primary group | `usermod -g developers user` |
| `-L` | Lock account | `usermod -L user` |
| `-U` | Unlock account | `usermod -U user` |

### The `userdel` Command - Delete User

Remove user accounts:

```bash
# Delete user (keep home directory)
sudo userdel username

# Delete user and home directory
sudo userdel -r username

# Force delete (even if logged in)
sudo userdel -f username

# Delete user, home, and mail spool
sudo userdel -r username
```

**⚠️ Warning:** Deleting a user is permanent. Backup important data first!

### Change Password Policies

```bash
# Change password expiration
sudo chage -M 90 username    # Must change every 90 days

# Set minimum days between changes
sudo chage -m 7 username     # Wait 7 days before changing again

# Set warning days
sudo chage -W 14 username    # Warn 14 days before expiration

# View password aging info
sudo chage -l username

# Force password change on next login
sudo chage -d 0 username
```

---

## 👨‍💼 Creating Admin Users

### Adding User to Sudo Group

Give a user administrative privileges:

```bash
# Add existing user to sudo group
sudo usermod -aG sudo username

# On RHEL/CentOS, use wheel group
sudo usermod -aG wheel username

# Verify user is in sudo group

groups username
```

### Create New Admin User (Complete Process)

```bash
# 1. Create user with home directory
sudo useradd -m -s /bin/bash -c "Admin User" adminuser

# 2. Set password
sudo passwd adminuser

# 3. Add to sudo group
sudo usermod -aG sudo adminuser

# 4. Verify
groups adminuser
id adminuser

# 5. Test sudo access (as the new user)
su - adminuser
sudo whoami    # Should output: root
```

### Configure Sudo Access (Advanced)

Edit sudo configuration:

```bash
# Edit sudoers file (always use visudo!)
sudo visudo

# Grant specific user all sudo privileges
username ALL=(ALL:ALL) ALL

# Grant passwordless sudo
username ALL=(ALL) NOPASSWD:ALL

# Grant specific commands only
username ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl
```

**⚠️ Important:** Always use `visudo` to edit `/etc/sudoers` - it checks syntax before saving!

---

## 🏢 Group Management

### Creating Groups

```bash
# Create new group
sudo groupadd groupname

# Create group with specific GID
sudo groupadd -g 1500 groupname

# Example:
sudo groupadd developers
sudo groupadd -g 2000 projects
```

### Modifying Groups

```bash
# Change group name
sudo groupmod -n newname oldname

# Change GID
sudo groupmod -g 2500 groupname

# Add user to group
sudo usermod -aG groupname username

# Remove user from group (set all groups, exclude one)
sudo gpasswd -d username groupname
```

### Deleting Groups

```bash
# Delete group
sudo groupdel groupname

# Note: Cannot delete a user's primary group
```

### Managing Group Members

```bash
# Add user to group
sudo gpasswd -a username groupname

# Remove user from group
sudo gpasswd -d username groupname

# Set group administrators
sudo gpasswd -A admin_user groupname

# View group members
getent group groupname
```

---

## 📊 User Account Monitoring

### View Logged-In Users

```bash
# Show currently logged-in users
who

# Show current user
whoami

# Detailed info about logged-in users
w

# Show login history
last

# Show last logins per user
lastlog

# Show failed login attempts
sudo lastb
```

### Check User Information

```bash
# View user details
id username
finger username        # If installed
getent passwd username

# View when password expires
sudo chage -l username

# View user's groups
groups username

# Check if user account is locked
sudo passwd -S username
# Output: username P = usable, L = locked, NP = no password
```

### User Activity Monitoring

```bash
# Show who is logged in and what they're doing
w

# Show login history
last username

# Show recent logins
lastlog

# Show failed login attempts (requires root)
sudo lastb

# Show specific user's last login
lastlog -u username

# Check active user sessions
who -a
```

### Auditing Commands

```bash
# List all users
cut -d: -f1 /etc/passwd

# List all regular users (UID >= 1000)
awk -F: '$3 >= 1000 {print $1}' /etc/passwd

# List users with sudo access
getent group sudo

# Count total users
wc -l /etc/passwd

# Find users with no password
sudo awk -F: '($2 == "") {print $1}' /etc/shadow

# Find locked accounts
sudo passwd -S --all | grep ' L '

# Check for duplicate UIDs
cut -d: -f3 /etc/passwd | sort | uniq -d
```

---

## 🛡️ User Security Best Practices

1. **Use strong passwords**: Enforce password policies with `pam_pwquality`
2. **Limit sudo access**: Only give admin rights to trusted users
3. **Disable unused accounts**: Lock or delete accounts not in use
4. **Regular audits**: Check for unauthorized users and suspicious activity
5. **Use SSH keys**: Prefer SSH keys over passwords for remote access
6. **Set password expiration**: Force regular password changes
7. **Monitor logs**: Check `/var/log/auth.log` for suspicious activity
8. **Disable root login**: Use sudo instead of logging in as root

```bash
# Lock unused account
sudo usermod -L username

# Set password expiration
sudo chage -M 90 username

# Disable root login via SSH
# Edit /etc/ssh/sshd_config:
# PermitRootLogin no
```

---

## 📋 Quick Reference Commands

```bash
# User Operations
sudo useradd -m -s /bin/bash username    # Create user
sudo passwd username                      # Set password
sudo usermod -aG sudo username           # Add to sudo group
sudo userdel -r username                 # Delete user with home

# Group Operations
sudo groupadd groupname                  # Create group
sudo usermod -aG groupname username      # Add user to group
sudo gpasswd -d username groupname       # Remove from group
sudo groupdel groupname                  # Delete group

# Information
id username                              # User ID and groups
groups username                          # List user's groups
who                                      # Logged-in users
last                                     # Login history
sudo chage -l username                   # Password info
```

---
