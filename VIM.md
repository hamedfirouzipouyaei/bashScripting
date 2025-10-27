# 📝 VIM Editor Reference Guide

VIM (Vi IMproved) is a powerful, modal text editor that's essential for Linux system administration and development.

---

## 🎯 VIM Modes Overview

VIM operates in three primary modes:

* **Command Mode** — Default mode for navigation and text manipulation
* **Insert Mode** — For typing and editing text
* **Last Line Mode** — For executing commands, saving, and quitting

**VIM Configuration File:** `~/.vimrc`

---

## ✏️ Entering Insert Mode

Switch from **Command Mode** to **Insert Mode** using these keys:

| Key | Action | Description |
|-----|--------|-------------|
| `i` | Insert before cursor | Start typing at current cursor position |
| `I` | Insert at beginning of line | Move to start of line and enter insert mode |
| `a` | Insert after cursor | Move cursor one position right and insert |
| `A` | Insert at end of line | Move to end of line and enter insert mode |
| `o` | Insert on next line | Create new line below and enter insert mode |
| `O` | Insert on previous line | Create new line above and enter insert mode |

---

## 💻 Last Line Mode

### Entering Last Line Mode

From **Command Mode**, press `:` to enter **Last Line Mode**.

### Essential Last Line Commands

| Command | Action | Description |
|---------|--------|-------------|
| `:w!` | Write/Save | Save the file (force write) |
| `:q!` | Quit without saving | Exit without saving changes |
| `:wq!` | Save and quit | Write file and exit |
| `:e!` | Reload file | Undo all changes to last saved version |
| `:set nu` | Show line numbers | Display line numbers |
| `:set nonu` | Hide line numbers | Remove line number display |
| `:syntax on` | Enable syntax highlighting | Turn on syntax coloring |
| `:syntax off` | Disable syntax highlighting | Turn off syntax coloring |

### Search and Replace

```bash
# Replace all occurrences in the file
:%s/search_string/replace_string/g

# Replace in current line only
:s/search_string/replace_string/g

# Replace with confirmation
:%s/search_string/replace_string/gc
```

---

## ⌨️ Command Mode Shortcuts

### Returning to Command Mode

Press `ESC` to return to **Command Mode** from any other mode.

### Text Editing

| Key | Action | Description |
|-----|--------|-------------|
| `x` | Delete character | Remove character under cursor |
| `dd` | Delete line | Cut/delete current line |
| `5dd` | Delete 5 lines | Cut/delete 5 lines from cursor |
| `u` | Undo | Undo last change |
| `Ctrl+r` | Redo | Redo last undone change |

### Copy and Paste

| Key | Action | Description |
|-----|--------|-------------|
| `y` | Yank/Copy | Copy selected text to clipboard |
| `yy` | Copy line | Copy current line |
| `5yy` | Copy 5 lines | Copy 5 lines from cursor |
| `p` | Paste after | Paste clipboard content after cursor |
| `P` | Paste before | Paste clipboard content before cursor |

### Navigation

| Key | Action | Description |
|-----|--------|-------------|
| `G` | Go to end of file | Move cursor to last line |
| `gg` | Go to beginning of file | Move cursor to first line |
| `$` | End of line | Move to end of current line |
| `0` or `^` | Beginning of line | Move to start of current line |
| `:n` | Go to line n | Move to specific line number (e.g., `:10`) |
| `w` | Next word | Move to beginning of next word |
| `b` | Previous word | Move to beginning of previous word |

### Selection

| Key | Action | Description |
|-----|--------|-------------|
| `Shift+v` | Select line | Select entire current line |
| `v` | Visual mode | Start character-wise selection |
| `Ctrl+v` | Visual block | Start block selection |

### Search

| Key | Action | Description |
|-----|--------|-------------|
| `/string` | Search forward | Search for "string" from cursor down |
| `?string` | Search backward | Search for "string" from cursor up |
| `n` | Next occurrence | Move to next search result |
| `N` | Previous occurrence | Move to previous search result |

### Save and Exit

| Key | Action | Description |
|-----|--------|-------------|
| `ZZ` | Save and quit | Quick save and exit (same as `:wq`) |
| `ZQ` | Quit without saving | Quick exit without saving (same as `:q!`) |

---

## 🔀 Working with Multiple Files

### Opening Multiple Files

```bash
# Open files in horizontal split windows
vim -o file1 file2

# Open files in vertical split windows  
vim -O file1 file2

# Compare files side by side (diff mode)
vim -d file1 file2
```

### Window Navigation

| Key | Action | Description |
|-----|--------|-------------|
| `Ctrl+w` then `w` | Switch windows | Move between open files |
| `Ctrl+w` then `h/j/k/l` | Navigate windows | Move left/down/up/right between windows |
| `Ctrl+w` then `q` | Close window | Close current window |
| `Ctrl+w` then `o` | Close others | Close all windows except current |

### File Management

```bash
# Open new file in current window
:e filename

# Open file in new horizontal split
:sp filename

# Open file in new vertical split
:vsp filename

# List open buffers
:ls

# Switch to buffer number n
:bn
```

---

## 🚀 Quick Reference Summary

### Mode Transitions

```text
Command Mode ←--[ESC]--→ Insert Mode (i,a,o,etc.)
     ↓                        ↑
     :                      [ESC]
     ↓                        ↑
 Last Line Mode ←--[ESC]--→
```

### Most Used Commands

```bash
# Essential survival commands
:w          # Save
:q          # Quit
:wq         # Save and quit
:q!         # Quit without saving
u           # Undo
dd          # Delete line
/search     # Search
n           # Next search result
```

---

## 💡 Pro Tips

1. **Always know your mode** - Check the bottom of the screen
2. **Use `ESC` frequently** - When in doubt, press ESC to get to Command Mode
3. **Practice navigation** - Learn `h,j,k,l` for cursor movement
4. **Master search** - `/` and `?` are your friends for quick navigation
5. **Use visual mode** - `v` for precise text selection
6. **Learn the dot command** - `.` repeats your last action
7. **Customize your `~/.vimrc`** - Make VIM work the way you want

---
