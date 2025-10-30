# ⛴️ [🦬💩] The Most Annoying Skill That You Should Have 😫

## What is Docker? 🤔

Docker is a platform that packages applications and their dependencies into **containers** - lightweight, portable units that run consistently across different environments.

**📣 Important Disclaimer:** The above statement is what I learned about Docker. My personal opinion is that Docker is the most unnecessary and yet essential tool that I have used in my entire life. It really pushes you to the edge of existence.

**Think of it like this:**

- Traditional way: "It works on my machine!" 😤
- Docker way: "It works in my container, so it works everywhere!" 😎

**Key Concepts:**

- **Image**: A blueprint/template for your application (like a class)
- **Container**: A running instance of an image (like an object)
- **Dockerfile**: Recipe for building an image
- **Registry**: Storage for images (Docker Hub is like GitHub for containers)

---

## Installation 📦

### Ubuntu/Debian

```bash
# Update package index
sudo apt update

# Install prerequisites
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Community Edition (ce)
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
sudo docker --version
```

### Add Your User to Docker Group (Avoid using sudo)

```bash
# Add current user to docker group
sudo usermod -aG docker $USER

# Apply changes (logout/login or use)
newgrp docker

# Test without sudo
docker run hello-world
```

---

## Basic Docker Commands 🚀

### Image Management

```bash
# List all images
docker images

# Pull an image from Docker Hub
docker pull ubuntu:22.04

# Build an image from Dockerfile
docker build -t myapp:1.0 .

# Remove an image
docker rmi image_name

# Remove all unused images
docker image prune -a
```

### Container Management

```bash
# Run a container
docker run ubuntu:22.04

# Run container interactively
docker run -it ubuntu:22.04 /bin/bash

# Run container in background (detached)
docker run -d nginx

# Run with port mapping
docker run -p 8080:80 nginx

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop container_id

# Start a stopped container
docker start container_id

# Remove a container
docker rm container_id

# Remove all stopped containers
docker container prune
```

### Useful Commands

```bash
# View container logs
docker logs container_id

# Execute command in running container
docker exec -it container_id /bin/bash

# Copy files from container
docker cp container_id:/path/in/container /host/path

# Copy files to container
docker cp /host/path container_id:/path/in/container

# Inspect container details
docker inspect container_id

# View container resource usage
docker stats
```

---

## Creating a Dockerfile 📝

### Simple C++ Example

**Project Structure:**

```text
my-cpp-app/
├── Dockerfile
├── dockerbuild.sh
├── dockerrun.sh
├── src/
│   └── main.cpp
└── CMakeLists.txt
```

**src/main.cpp:**

```cpp
#include <iostream>

int main() {
    std::cout << "Hello from Docker!" << std::endl;
    return 0;
}
```

**Dockerfile:**

```dockerfile
# Use Ubuntu as base image
FROM ubuntu:22.04

# Avoid interactive prompts during build
ENV DEBIAN_FRONTEND=noninteractive

# Install build tools
RUN apt-get update && apt-get install -y \
    g++ \
    cmake \
    make \
    gdb \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy source code
COPY src/ ./src/
COPY CMakeLists.txt .

# Build the application
RUN cmake . && make

# Run the application
CMD ["./my_program"]
```

**CMakeLists.txt:**

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyApp)

set(CMAKE_CXX_STANDARD 17)

add_executable(my_program src/main.cpp)
```

---

## Efficient Build and Run Scripts 🛠️

### dockerbuild.sh

Create a script to build your Docker image efficiently:

```bash
#!/bin/bash

# dockerbuild.sh - Build Docker image

IMAGE_NAME="mycppapp"
IMAGE_TAG="latest"

echo "🔨 Building Docker image: ${IMAGE_NAME}:${IMAGE_TAG}"

docker build \
    -t ${IMAGE_NAME}:${IMAGE_TAG} \
    --build-arg BUILD_DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ') \
    .

if [ $? -eq 0 ]; then
    echo "✅ Build successful!"
    echo "📦 Image: ${IMAGE_NAME}:${IMAGE_TAG}"
else
    echo "❌ Build failed!"
    exit 1
fi
```

**Make it executable:**

```bash
chmod +x dockerbuild.sh
```

### dockerrun.sh

Create a script to run your container with common options:

```bash
#!/bin/bash

# dockerrun.sh - Run Docker container

IMAGE_NAME="mycppapp"
IMAGE_TAG="latest"
CONTAINER_NAME="mycppapp-container"

echo "🚀 Running Docker container: ${CONTAINER_NAME}"

# Remove old container if exists
docker rm -f ${CONTAINER_NAME} 2>/dev/null

# Run container
docker run \
    --name ${CONTAINER_NAME} \
    -v $(pwd)/src:/app/src \
    -it \
    ${IMAGE_NAME}:${IMAGE_TAG}

# Cleanup
docker rm ${CONTAINER_NAME} 2>/dev/null
```

**Make it executable:**

```bash
chmod +x dockerrun.sh
```

### Advanced dockerrun.sh with Options

```bash
#!/bin/bash

# dockerrun.sh - Run Docker container with options

IMAGE_NAME="mycppapp"
IMAGE_TAG="latest"
CONTAINER_NAME="mycppapp-container"

# Parse command line arguments
MOUNT_SOURCE=false
INTERACTIVE=true
COMMAND=""

while [[ $# -gt 0 ]]; do
    case $1 in
        -m|--mount)
            MOUNT_SOURCE=true
            shift
            ;;
        -d|--detached)
            INTERACTIVE=false
            shift
            ;;
        -c|--command)
            COMMAND="$2"
            shift 2
            ;;
        *)
            echo "Unknown option: $1"
            exit 1
            ;;
    esac
done

# Build docker run command
RUN_CMD="docker run --name ${CONTAINER_NAME}"

# Add volume mount if requested
if [ "$MOUNT_SOURCE" = true ]; then
    RUN_CMD="$RUN_CMD -v $(pwd)/src:/app/src"
fi

# Add interactive flag
if [ "$INTERACTIVE" = true ]; then
    RUN_CMD="$RUN_CMD -it"
else
    RUN_CMD="$RUN_CMD -d"
fi

# Add image
RUN_CMD="$RUN_CMD ${IMAGE_NAME}:${IMAGE_TAG}"

# Add custom command if provided
if [ -n "$COMMAND" ]; then
    RUN_CMD="$RUN_CMD $COMMAND"
fi

# Remove old container if exists
docker rm -f ${CONTAINER_NAME} 2>/dev/null

# Run container
echo "🚀 Running: $RUN_CMD"
eval $RUN_CMD

# Cleanup after interactive session
if [ "$INTERACTIVE" = true ]; then
    docker rm ${CONTAINER_NAME} 2>/dev/null
fi
```

**Usage examples:**

```bash
# Run normally
./dockerrun.sh

# Run with source mounted (for development)
./dockerrun.sh --mount

# Run in background
./dockerrun.sh --detached

# Run custom command
./dockerrun.sh --command "/bin/bash"
```

---

### Volume Mounting (Shared Folders) 📁

Volume mounting allows you to **share files and directories** between your host machine and Docker containers. This is essential for:

- **Development**: Edit files on host, see changes in container immediately
- **Data persistence**: Keep data even when container is deleted
- **Configuration**: Share config files between host and container
- **Output**: Get results from container back to host

#### Basic Volume Mounting Syntax

```bash
# Basic syntax
docker run -v /host/path:/container/path image_name

# Multiple volumes
docker run -v /host/path1:/container/path1 -v /host/path2:/container/path2 image_name

# Read-only mount
docker run -v /host/path:/container/path:ro image_name
```

#### Advanced Example: Complete Development Environment

Here's a real-world example that demonstrates advanced volume mounting with GPU support, GUI applications, and multiple shared folders:

```bash
#!/bin/bash
set -ex

# Clean up any existing container named hfp_dock
docker rm -f hfp_dock 2>/dev/null || true

# Define the IMAGE you want to run
IMAGE_NAME="hfp_dock:with-mathlib"

# GPU support (set to "--gpus all" to enable GPU, empty to disable)
# Note: Requires nvidia-container-toolkit to be installed
GPU_FLAG="${GPU_FLAG:-}"

# Start the container with RH project mounted for development
docker run \
    --rm \
    -ti \
    ${GPU_FLAG} \
    --privileged \
    --env=DISPLAY=$DISPLAY \
    --env=XAUTHORITY=/tmp/.Xauthority \
    --volume=/home/hfp/Docker_Shared_Folder/:/home/hfp/docs \
    --volume=/home/hfp/Github/RH:/workspace/RH \
    --volume=$XAUTHORITY:/tmp/.Xauthority:rw \
    --volume=/tmp/.X11-unix:/tmp/.X11-unix \
    --name=hfp_dock \
    --hostname=hfp_dock \
    --network=host \
    --add-host=hfp_dock:127.0.1.1 \
    --workdir=/workspace/RH \
    "${IMAGE_NAME}" \
    "$@"
```

#### Breaking Down the Advanced Example

**Volume Mounts Explained:**

```bash
# 1. Shared documents folder
--volume=/home/hfp/Docker_Shared_Folder/:/home/hfp/docs
# Maps: Host folder → Container folder
#       /home/hfp/Docker_Shared_Folder/ → /home/hfp/docs
# Purpose: Share documents, files, and data between host and container

# 2. Development project folder
--volume=/home/hfp/Github/RH:/workspace/RH  
# Maps: Host project → Container workspace
#       /home/hfp/Github/RH → /workspace/RH
# Purpose: Edit code on host, compile/run in container

# 3. X11 authorization (for GUI apps)
--volume=$XAUTHORITY:/tmp/.Xauthority:rw
# Maps: Host X11 auth → Container X11 auth (read-write)
# Purpose: Allow GUI applications to run from container

# 4. X11 socket (for GUI apps)
--volume=/tmp/.X11-unix:/tmp/.X11-unix
# Maps: Host X11 socket → Container X11 socket  
# Purpose: Enable graphical display forwarding
```

**Other Important Flags:**

```bash
# Container lifecycle
--rm                    # Remove container when it exits
-ti                     # Interactive terminal
--name=hfp_dock        # Give container a name
--hostname=hfp_dock    # Set container hostname

# Permissions and capabilities
--privileged           # Give extended privileges (use carefully!)

# Environment variables
--env=DISPLAY=$DISPLAY              # Forward display variable
--env=XAUTHORITY=/tmp/.Xauthority   # Set X11 authorization

# Networking
--network=host                      # Use host network
--add-host=hfp_dock:127.0.1.1      # Add custom host entry

# Working directory
--workdir=/workspace/RH             # Set starting directory in container

# GPU support (optional)
${GPU_FLAG}                         # Add "--gpus all" for GPU access
```

#### Volume Types and Use Cases

**1. Bind Mounts** (Most Common)

```bash
# Bind mount - direct path mapping
docker run -v /home/user/project:/app/project image_name
```

**2. Named Volumes** (Docker Managed)

```bash
# Create named volume
docker volume create my-data

# Use named volume
docker run -v my-data:/app/data image_name

# List volumes
docker volume ls

# Remove volume
docker volume rm my-data
```

**3. Anonymous Volumes** (Temporary)

```bash
# Anonymous volume (Docker creates random name)
docker run -v /app/data image_name
```

#### Practical Volume Mounting Examples

**Development Workflow:**

```bash
# Mount current directory for live editing
docker run -v $(pwd):/workspace -w /workspace gcc:latest g++ main.cpp -o app

# Mount home directory for access to all files
docker run -v $HOME:/home/user ubuntu:22.04
```

**Data Processing:**

```bash
# Input and output folders
docker run \
    -v /host/input:/app/input:ro \
    -v /host/output:/app/output \
    data-processor:latest
```

**Configuration Sharing:**

```bash
# Share configuration files
docker run \
    -v /host/config:/app/config:ro \
    -v /host/logs:/app/logs \
    web-server:latest
```

#### 💡 Volume Mounting Best Practices

1. **Use absolute paths**: Always use full paths for reliability
2. **Read-only when possible**: Add `:ro` for config files you don't want to modify
3. **Organize your mounts**: Group related volumes together
4. **Use named volumes for data**: Better for databases and persistent storage
5. **Be careful with permissions**: Container user might not match host user
6. **Avoid mounting sensitive directories**: Don't mount `/` or system directories

#### 🚨 Common Volume Mounting Issues

**Permission Problems:**

```bash
# Fix permission issues by matching user IDs
docker run -u $(id -u):$(id -g) -v $(pwd):/workspace image_name

# Or set proper ownership after
sudo chown -R $USER:$USER /host/mounted/folder
```

**Path Issues:**

```bash
# ❌ Wrong: relative paths can be unreliable
docker run -v ./code:/app/code image_name

# ✅ Correct: use absolute paths
docker run -v $(pwd)/code:/app/code image_name
```

## Using Docker as Compiler in VS Code 💻

### Method 1: Docker Extension

**Install Docker Extension:**

```bash
code --install-extension ms-azuretools.vscode-docker
```

### Method 2: Configure Tasks to Use Docker

**Modify `.vscode/tasks.json`:**

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Docker: Build C++ in Container",
            "type": "shell",
            "command": "docker",
            "args": [
                "run",
                "--rm",
                "-v",
                "${workspaceFolder}:/app",
                "-w",
                "/app",
                "gcc:latest",
                "g++",
                "-g",
                "-Wall",
                "-std=c++17",
                "${relativeFile}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": ["$gcc"]
        },
        {
            "label": "Docker: Run C++ Program",
            "type": "shell",
            "command": "docker",
            "args": [
                "run",
                "--rm",
                "-v",
                "${workspaceFolder}:/app",
                "-w",
                "/app",
                "gcc:latest",
                "./${relativeFileDirname}/${fileBasenameNoExtension}"
            ],
            "dependsOn": ["Docker: Build C++ in Container"]
        }
    ]
}
```

**Explanation:**

- **--rm**: Automatically remove container after execution
- **-v ${workspaceFolder}:/app**: Mount workspace to /app in container
- **-w /app**: Set working directory
- **gcc:latest**: Use official GCC Docker image

### Method 3: Dev Containers

**Install Dev Containers Extension:**

```bash
code --install-extension ms-vscode-remote.remote-containers
```

**Create `.devcontainer/devcontainer.json`:**

```json
{
    "name": "C++ Development",
    "image": "mcr.microsoft.com/devcontainers/cpp:ubuntu-22.04",
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-vscode.cpptools",
                "ms-vscode.cmake-tools"
            ],
            "settings": {
                "C_Cpp.default.cppStandard": "c++17"
            }
        }
    },
    "postCreateCommand": "apt-get update && apt-get install -y gdb cmake"
}
```

**Open in Dev Container:**

1. Press `Ctrl+Shift+P`
2. Search "Dev Containers: Reopen in Container"
3. VS Code will reopen inside the container
4. All development happens in the container!

---

## Using Docker as Compiler in CLion 🔧

### Configure Docker Toolchain

#### Step 1: Enable Docker Plugin

1. Go to `File` → `Settings` → `Plugins`
2. Search for "Docker" and enable it
3. Restart CLion if needed

#### Step 2: Add Docker as Toolchain

1. Go to `File` → `Settings` → `Build, Execution, Deployment` → `Toolchains`
2. Click `+` → Select `Docker`
3. Configure:
   - **Server**: Docker (should auto-detect)
   - **Image**: gcc:latest (or your custom image)
   - **CMake**: Detect automatically
   - **Build Tool**: Detect automatically
   - **C Compiler**: Detect automatically
   - **C++ Compiler**: Detect automatically
   - **Debugger**: gdb

#### Step 3: Configure CMake Profile

1. Go to `File` → `Settings` → `Build, Execution, Deployment` → `CMake`
2. Click `+` to add new profile
3. Configure:
   - **Name**: Docker Debug
   - **Build type**: Debug
   - **Toolchain**: Docker (the one you just created)
   - **CMake options**: `-DCMAKE_BUILD_TYPE=Debug`

#### Step 4: Select Profile and Build

1. In the toolbar, select "Docker Debug" profile
2. Click Build button or press `Ctrl+F9`
3. CLion will build inside the Docker container!

### Custom Dockerfile for CLion

**Dockerfile.clion:**

```dockerfile
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install development tools
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    gdb \
    git \
    ninja-build \
    && rm -rf /var/lib/apt/lists/*

# Set up workspace
WORKDIR /workspace

CMD ["/bin/bash"]
```

**Build and use:**

```bash
docker build -t clion-dev:latest -f Dockerfile.clion .
```

Then in CLion settings, use `clion-dev:latest` as the image.

---

## Docker Best Practices 🎯

### 1. Use .dockerignore

Create `.dockerignore` to exclude files from build context:

```text
# .dockerignore
build/
.git/
.vscode/
*.o
*.a
*.so
node_modules/
```

### 2. Multi-stage Builds (Smaller Images)

```dockerfile
# Build stage
FROM gcc:latest AS builder
WORKDIR /app
COPY src/ ./src/
COPY CMakeLists.txt .
RUN cmake . && make

# Runtime stage
FROM ubuntu:22.04
WORKDIR /app
COPY --from=builder /app/my_program .
CMD ["./my_program"]
```

### 3. Layer Caching (Faster Builds)

```dockerfile
# Install dependencies first (rarely changes)
RUN apt-get update && apt-get install -y g++ cmake

# Copy dependency files next
COPY CMakeLists.txt .

# Copy source last (changes frequently)
COPY src/ ./src/
```

### 4. Use Specific Tags

```dockerfile
# ❌ Bad: version changes unexpectedly
FROM ubuntu:latest

# ✅ Good: explicit version
FROM ubuntu:22.04
```

---

## Quick Reference 📖

### Common Docker Commands

| Command | Description |
|---------|-------------|
| `docker build -t name .` | Build image |
| `docker run -it name` | Run interactively |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker stop id` | Stop container |
| `docker rm id` | Remove container |
| `docker rmi name` | Remove image |
| `docker logs id` | View logs |
| `docker exec -it id bash` | Enter container |

### Dockerfile Instructions

| Instruction | Description |
|-------------|-------------|
| `FROM` | Base image |
| `RUN` | Execute command during build |
| `COPY` | Copy files to image |
| `WORKDIR` | Set working directory |
| `ENV` | Set environment variable |
| `EXPOSE` | Document exposed ports |
| `CMD` | Default command to run |
| `ENTRYPOINT` | Configure container as executable |

---

## Troubleshooting 🔍

### Permission Denied

```bash
# Add user to docker group
sudo usermod -aG docker $USER
newgrp docker
```

### Container Exits Immediately

```bash
# Run with interactive terminal
docker run -it image_name /bin/bash
```

### Can't Connect to Docker Daemon

```bash
# Start Docker service
sudo systemctl start docker
sudo systemctl status docker
```

### Remove All Containers and Images

```bash
# Stop all containers
docker stop $(docker ps -aq)

# Remove all containers
docker rm $(docker ps -aq)

# Remove all images
docker rmi $(docker images -q)
```

---

## Additional Resources 📚

- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers)

---

**Remember:** Docker might be annoying at first, but once you master it, you'll wonder how you ever lived without it! 🚀🐳 [Just kidding, it will still annoy you for all eternity 🤣😂🤣😂🤣]

> **📌 Important Note:** Dockerfiles are written based on Linux commands, and as the extension indicates, the build and run files are written with Bash. For more information, you can refer to the **Linux Essential References** chapter for Dockerfile and the **Bash Scripting** chapter for build and run files.
