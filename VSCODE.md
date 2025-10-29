# 🖇 VS Code Setup on Linux [Ubuntu]

> 📢❗🚨 **Important Note:** Make sure your compiler is already installed on your host machine.

```bash
# Check if you have compiler installed on your machine
g++ --version

# Otherwise, install the following (for Ubuntu distribution)
sudo apt install g++ gcc -y
```

## Install VS Code

```bash
# Fastest way to install
sudo apt install code -y
```

### Alternative Installation Methods

```bash
# Install via Snap
sudo snap install --classic code

# Or download from Microsoft's official website
# Visit: https://code.visualstudio.com/Download
```

## Install Necessary Extensions

You can navigate to the Extensions tab and install the `C/C++` extension:

![VSCode C++ Extension](images/vscode-cpp-extension.png)

### Installing the C/C++ Extension

1. Open VS Code
2. Click on the Extensions icon in the sidebar (or press `Ctrl+Shift+X`)
3. Search for "C/C++"
4. Install the extension by Microsoft (should be the first result)

Alternatively, you can install it via command line:

```bash
code --install-extension ms-vscode.cpptools
```
