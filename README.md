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

## 🔄 Piping and Command Redirection

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
