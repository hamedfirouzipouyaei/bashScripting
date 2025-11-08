# 💡 Useful Hints and Tips

## Removing Installed Library Files 🗑️

When you install a library (typically using `make install`), most build systems will create a file named `install_manifest.txt` in the build directory. This file contains a list of all the files that were installed on your host machine.

The best way to remove all the installed library files that have affected your host machine is to use the content of this file and remove them with the following command:

```bash
# GNU xargs (Linux)
xargs -a install_manifest.txt -d '\n' sudo rm -vf
```

**What this command does:**

- `xargs -a install_manifest.txt`: Read file paths from the install_manifest.txt file
- `-d '\n'`: Use newline as delimiter (each file path is on a separate line)
- `sudo rm -vf`: Remove files with verbose output and force removal
- `-v`: Verbose output (shows which files are being removed)
- `-f`: Force removal (don't prompt for confirmation)

**Example usage:**

```bash
# Navigate to your build directory
cd build/

# Check if install_manifest.txt exists
ls -la install_manifest.txt

# Remove all installed files
xargs -a install_manifest.txt -d '\n' sudo rm -vf

# From inside a docker
xargs -a install_manifest.txt -d '\n' rm -vf
```

**⚠️ Warning:** This command will permanently delete files from your system. Make sure you're in the correct build directory and that the `install_manifest.txt` file corresponds to the library you want to uninstall.

**💡 Tip:** You can preview which files will be removed by first running:

```bash
cat install_manifest.txt
```

This will show you all the files that were installed, so you can verify before deletion.
