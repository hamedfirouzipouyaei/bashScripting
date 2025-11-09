# 🧨 CUDA Parallel Programming Cheat Sheet

## 📌 Introduction to NVIDIA GPUs

### 🌀 The Evolution of NVIDIA Graphics Processing Units

NVIDIA Corporation, founded in 1993, revolutionized computing by transforming GPUs from specialized graphics processors into powerful parallel computing platforms. The introduction of CUDA (Compute Unified Device Architecture) in 2006 marked a pivotal moment, enabling developers to harness GPU parallelism for general-purpose computing.

### 🏛️ GPU Architecture Generations

NVIDIA's GPU architectures have evolved significantly over the past two decades, each generation bringing substantial improvements in performance, efficiency, and programmability.

#### 🗓 Architecture Timeline

| Architecture | Year | Key Features | Compute Capability | Notable GPUs |
|--------------|------|--------------|-------------------|--------------|
| **Tesla** | 2006 | First CUDA-enabled architecture, unified shader model | 1.0 - 1.3 | GeForce 8800 GTX, Tesla C870 |
| **Ferris** | 2010 | ECC memory, L1/L2 cache, concurrent kernels | 2.0 - 2.1 | GeForce GTX 480, Tesla C2050 |
| **Kepler** | 2012 | Dynamic parallelism, Hyper-Q, GPU Direct | 3.0 - 3.7 | GeForce GTX 680, Tesla K40 |
| **Maxwell** | 2014 | Improved power efficiency, shared memory | 5.0 - 5.3 | GeForce GTX 980, GTX 750 Ti |
| **Pascal** | 2016 | NVLink, HBM2 memory, unified memory | 6.0 - 6.2 | GeForce GTX 1080, Tesla P100 |
| **Volta** | 2017 | Tensor cores, independent thread scheduling | 7.0 - 7.2 | Tesla V100, Titan V |
| **Turing** | 2018 | RT cores for ray tracing, mesh shaders | 7.5 | GeForce RTX 2080, Quadro RTX |
| **Ampere** | 2020 | 2nd gen tensor cores, 3rd gen NVLink | 8.0 - 8.6 | GeForce RTX 3090, A100 |
| **Ada Lovelace** | 2022 | 4th gen tensor cores, DLSS 3.0 | 8.9 | GeForce RTX 4090, RTX 4080 |
| **Hopper** | 2022 | Transformer engine, DPX instructions | 9.0 | H100, H200 |
| **Blackwell** | 2024 | 2nd gen transformer engine, 5th gen NVLink | 10.0 (est.) | B100, B200 |

### 💡 Major Architecture Innovations

#### ⚡ Tesla Architecture (2006-2009)

- **Revolutionary Change**: First unified shader architecture enabling GPGPU computing
- **CUDA Introduction**: Parallel computing platform and programming model
- **Specifications**: 128 streaming processors (SP), 768 MHz core clock
- **Impact**: Democratized parallel computing for scientific and research applications

#### ⚛ Fermi Architecture (2010-2012)

- **Memory Hierarchy**: Introduced configurable L1 cache and unified L2 cache
- **Error Correction**: ECC memory support for mission-critical applications
- **Concurrent Execution**: Up to 16 simultaneous kernel executions
- **Compute Capability 2.0**: Enabled C++ support and improved debugging

#### 🔭 Kepler Architecture (2012-2014)

- **Dynamic Parallelism**: GPU threads can launch new threads without CPU intervention
- **Hyper-Q**: 32 simultaneous hardware work queues (vs 1 in Fermi)
- **GPU Direct**: Direct peer-to-peer communication between GPUs
- **SMX Units**: Streaming Multiprocessor with 192 CUDA cores per SMX

#### 〽️ Maxwell Architecture (2014-2016)

- **Energy Efficiency**: 2x performance per watt compared to Kepler
- **SMM Design**: Streamlined multiprocessor with 128 CUDA cores per SMM
- **Memory Compression**: Reduced bandwidth requirements
- **Ideal For**: Deep learning inference and embedded systems

#### 🦎 Pascal Architecture (2016-2018)

- **NVLink**: High-bandwidth interconnect (160 GB/s vs PCIe's 16 GB/s)
- **HBM2 Memory**: High Bandwidth Memory with up to 732 GB/s bandwidth
- **Unified Memory**: Simplified programming with automatic page migration
- **16nm FinFET**: Smaller transistors, better power efficiency

#### 🔌 Volta Architecture (2017-2019)

- **Tensor Cores**: Specialized units for AI/ML with mixed-precision matrix operations
- **Independent Thread Scheduling**: More flexible execution and synchronization
- **Enhanced SM**: 64 FP32 cores + 64 INT32 cores + 8 Tensor cores per SM
- **Performance Leap**: 15 TFLOPs FP32, 120 TFLOPs mixed precision (V100)

#### 🌀 Turing Architecture (2018-2020)

- **RT Cores**: Hardware-accelerated ray tracing
- **2nd Gen Tensor Cores**: Improved AI inference performance
- **Mesh Shading**: Advanced geometry processing
- **Target Market**: Professional visualization and gaming

#### 🔋 Ampere Architecture (2020-2022)

- **Multi-Instance GPU (MIG)**: Partition single GPU into up to 7 isolated instances
- **3rd Gen Tensor Cores**: Sparsity support for 2x throughput
- **Enhanced FP64**: Doubled FP64 throughput for scientific computing
- **A100 Specs**: 6912 CUDA cores, 432 Tensor cores, 40/80 GB HBM2e

#### 🦗 Hopper Architecture (2022-Present)

- **Transformer Engine**: Optimized for large language models
- **4th Gen NVLink**: 900 GB/s bidirectional bandwidth
- **DPX Instructions**: 7x faster dynamic programming algorithms
- **H100 Specs**: 16896 CUDA cores, 528 Tensor cores, 80 GB HBM3

#### 🕳️ Blackwell Architecture (2024-Present)

- **208B Transistors**: Largest GPU ever built
- **2nd Gen Transformer Engine**: Enhanced FP4/FP6/FP8 precision
- **5th Gen NVLink**: 1.8 TB/s bidirectional bandwidth
- **B200 Specs**: 20,000 CUDA cores (est.), 192 GB HBM3e

### GPU Product Lines

NVIDIA segments its GPUs into distinct product lines for different market segments:

| Product Line | Target Market | Key Features | Example Models |
|--------------|---------------|--------------|----------------|
| **GeForce** | Gaming & Consumer | High graphics performance, RT cores | RTX 4090, RTX 4080, RTX 4070 |
| **Quadro/RTX** | Professional Visualization | Certified drivers, ECC memory | RTX 6000 Ada, RTX A6000 |
| **Tesla/Data Center** | HPC & Scientific Computing | Maximum compute, ECC, NVLink | A100, V100, P100 |
| **A-Series** | AI & Data Center | Tensor cores, MIG support | A100, A40, A30, A10 |
| **H-Series** | AI Training & HPC | Transformer engine, HBM3 | H100, H200 |
| **T-Series** | Inference & Edge | Power efficient, compact | T4, T1000 |
| **Jetson** | Embedded & Edge AI | Low power, integrated SoC | Jetson Orin, Jetson Xavier |

### Compute Capability Comparison

Compute capability defines the features supported by a CUDA-enabled GPU. Higher compute capability indicates newer architecture with more features.

| Compute Capability | Architecture | Key Features Available |
|-------------------|--------------|------------------------|
| **1.x** | Tesla | Basic CUDA, atomic operations on global memory |
| **2.x** | Fermi | C++ support, L1/L2 cache, atomic operations on shared memory |
| **3.x** | Kepler | Dynamic parallelism, Hyper-Q, warp shuffle |
| **5.x** | Maxwell | Dynamic parallelism improvements, better power efficiency |
| **6.x** | Pascal | Unified memory improvements, HBM2, NVLink |
| **7.x** | Volta/Turing | Tensor cores, independent thread scheduling, cooperative groups |
| **8.x** | Ampere/Ada | Multi-instance GPU, async copy, enhanced tensor cores |
| **9.x** | Hopper | Transformer engine, thread block clusters, DPX |
| **10.x** | Blackwell | 2nd gen transformer engine, enhanced precision modes |

### Memory Technology Evolution

| Memory Type | Bandwidth | Architecture Introduced | Capacity | Use Case |
|-------------|-----------|------------------------|----------|----------|
| **GDDR3** | ~86 GB/s | Tesla | 512 MB - 2 GB | Early CUDA GPUs |
| **GDDR5** | ~336 GB/s | Kepler/Maxwell | 2 GB - 12 GB | Consumer GPUs |
| **GDDR5X** | ~480 GB/s | Pascal | 8 GB - 12 GB | High-end gaming |
| **GDDR6** | ~760 GB/s | Turing/Ampere | 8 GB - 24 GB | Modern consumer GPUs |
| **GDDR6X** | ~1008 GB/s | Ampere/Ada | 12 GB - 24 GB | Flagship gaming GPUs |
| **HBM** | ~512 GB/s | Pascal (P100) | 16 GB | First gen high bandwidth |
| **HBM2** | ~900 GB/s | Pascal/Volta | 16 GB - 32 GB | Data center GPUs |
| **HBM2e** | ~2000 GB/s | Ampere | 40 GB - 80 GB | AI training (A100) |
| **HBM3** | ~3350 GB/s | Hopper | 80 GB - 141 GB | Latest data center (H100) |
| **HBM3e** | ~4800 GB/s | Blackwell | 192 GB | Next-gen AI (B200) |

### Performance Trends Across Generations

| GPU Model | Year | FP32 TFLOPs | FP64 TFLOPs | Tensor TFLOPs | Power (W) | Perf/Watt |
|-----------|------|-------------|-------------|---------------|-----------|-----------|
| Tesla C2050 | 2010 | 1.03 | 0.52 | - | 238 | 4.3 |
| Tesla K40 | 2013 | 4.29 | 1.43 | - | 235 | 18.3 |
| Tesla P100 | 2016 | 9.30 | 4.70 | - | 250 | 37.2 |
| Tesla V100 | 2017 | 15.7 | 7.80 | 125 | 300 | 52.3 |
| A100 (40GB) | 2020 | 19.5 | 9.70 | 312 | 400 | 48.8 |
| H100 (80GB) | 2022 | 51.0 | 25.6 | 1979 | 700 | 72.9 |
| B200 | 2024 | ~80.0 | ~40.0 | 4500 | 1000 | 80.0 |

*Note: Tensor TFLOP values are for FP16/BF16 precision. FP8 and lower precision operations offer even higher throughput.*

### Choosing the Right GPU for CUDA Development

#### For Learning & Development

- **Budget**: GTX 1650 or RTX 3050 (Compute Capability 7.5+)
- **Mid-Range**: RTX 3060/4060 (12 GB VRAM recommended)
- **Enthusiast**: RTX 3080/4080 (for complex projects)

#### For Professional/Research Work

- **Deep Learning**: A100, H100, or RTX 4090 (consumer alternative)
- **Scientific Computing**: A100, V100 (for FP64 performance)
- **Inference Deployment**: T4, A10 (power-efficient)

#### For Production/Enterprise

- **Large-Scale Training**: H100, H200, B100/B200
- **Multi-Tenant Environments**: A100/A30 with MIG support
- **Edge Deployment**: Jetson Orin, T4

### Key Considerations

1. **Compute Capability**: Ensure minimum CC 3.5+ for modern CUDA features
2. **Memory Capacity**: Match to your workload (8GB minimum, 16GB+ recommended)
3. **Memory Bandwidth**: Critical for memory-bound applications
4. **Tensor Cores**: Essential for deep learning (CC 7.0+)
5. **NVLink**: Multi-GPU scaling for large models (P100+)
6. **Power & Cooling**: Data center GPUs require substantial infrastructure

---

## 💾 Installing CUDA and Other Required Programs

### Prerequisites

Before installing CUDA, ensure you have:

- NVIDIA GPU with Compute Capability 3.5 or higher
- Supported Linux distribution (Ubuntu, RHEL, CentOS, etc.) or Windows
- GCC compiler (Linux) or Visual Studio (Windows)
- Sufficient disk space (~3-5 GB)

### Linux Installation

#### Ubuntu/Debian

```bash
# Check GPU and driver
lspci | grep -i nvidia
nvidia-smi

# Install NVIDIA Driver (if not installed)
sudo ubuntu-drivers devices
sudo ubuntu-drivers autoinstall

# Add CUDA Repository
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update

# Install CUDA Toolkit
sudo apt-get install cuda-toolkit-12-3

# Set Environment Variables
echo 'export PATH=/usr/local/cuda-12.3/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.3/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc

# Verify Installation
nvcc --version
```

#### RHEL/CentOS/Fedora

```bash
# Install NVIDIA Driver
sudo dnf install kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig

# Add CUDA Repository
sudo dnf config-manager --add-repo https://developer.download.nvidia.com/compute/cuda/repos/rhel9/x86_64/cuda-rhel9.repo

# Install CUDA
sudo dnf clean all
sudo dnf install cuda

# Set Environment Variables
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

### Windows Installation

1. Download CUDA Toolkit from [NVIDIA website](https://developer.nvidia.com/cuda-downloads)
2. Run the installer (choose Express or Custom installation)
3. Install Visual Studio 2019/2022 (required for NVCC compiler)
4. Verify installation:

```cmd
nvcc --version
nvidia-smi
```

### Essential Development Tools

#### CMake (Build System)

```bash
# Linux
sudo apt-get install cmake  # Ubuntu/Debian
sudo dnf install cmake      # RHEL/CentOS

# Verify
cmake --version
```

#### CUDA Samples

```bash
# Clone CUDA Samples
git clone https://github.com/NVIDIA/cuda-samples.git
cd cuda-samples/Samples/1_Utilities/deviceQuery
make
./deviceQuery
```

#### cuDNN (Deep Learning Library)

```bash
# Download from NVIDIA (requires account)
# https://developer.nvidia.com/cudnn

# Install on Linux
sudo dpkg -i cudnn-local-repo-*.deb
sudo cp /var/cudnn-local-repo-*/cudnn-local-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get install libcudnn8 libcudnn8-dev
```

#### NCCL (Multi-GPU Communication)

```bash
# Ubuntu/Debian
sudo apt install libnccl2 libnccl-dev

# Verify
ls /usr/lib/x86_64-linux-gnu/libnccl*
```

#### Nsight Tools (Profiling and Debugging)

Included with CUDA Toolkit:

- **Nsight Systems**: System-wide performance analysis
- **Nsight Compute**: Kernel-level profiler
- **Nsight Visual Studio Edition**: Windows debugging (Visual Studio integration)
- **cuda-gdb**: Command-line debugger

```bash
# Launch Nsight Systems
nsys

# Launch Nsight Compute
ncu

# Launch cuda-gdb
cuda-gdb ./program
```

### Version Compatibility

| CUDA Version | Minimum Driver | GCC (Linux) | Visual Studio (Windows) |
|--------------|----------------|-------------|------------------------|
| CUDA 12.3 | 545.23.06 | ≤ 12.x | 2019, 2022 |
| CUDA 12.0 | 525.60.13 | ≤ 12.x | 2019, 2022 |
| CUDA 11.8 | 520.61.05 | ≤ 11.x | 2017, 2019, 2022 |
| CUDA 11.0 | 450.80.02 | ≤ 10.x | 2017, 2019 |

### Verification Script

```bash
#!/bin/bash
echo "=== CUDA Installation Check ==="
echo "CUDA Compiler:"
nvcc --version
echo ""
echo "NVIDIA Driver:"
nvidia-smi
echo ""
echo "GPU Info:"
nvidia-smi --query-gpu=name,compute_cap,memory.total --format=csv
echo ""
echo "Environment Variables:"
echo "PATH: $PATH"
echo "LD_LIBRARY_PATH: $LD_LIBRARY_PATH"
```

---

## 📢 Introduction to CUDA

### What is CUDA?

**CUDA (Compute Unified Device Architecture)** is NVIDIA's parallel computing platform and programming model that enables dramatic increases in computing performance by harnessing the power of GPUs.

### CPU vs GPU Architecture

| Aspect | CPU | GPU |
|--------|-----|-----|
| **Cores** | Few (4-64) powerful cores | Thousands (1000s-10000s) of smaller cores |
| **Design Philosophy** | Minimize latency for sequential tasks | Maximize throughput for parallel tasks |
| **Clock Speed** | High (3-5 GHz) | Lower (1-2 GHz) |
| **Cache** | Large (MB per core) | Small (KB per core) |
| **Threads** | 10s-100s | Millions |
| **Best For** | Complex logic, branching | Data-parallel computations |
| **Control Logic** | Complex | Simple |

### CUDA Programming Model

#### Key Concepts

1. **Host**: The CPU and its memory (host memory)
2. **Device**: The GPU and its memory (device memory)
3. **Kernel**: Function that runs on the GPU
4. **Thread**: Basic unit of parallel execution
5. **Block**: Group of threads that can cooperate
6. **Grid**: Collection of blocks

### CUDA Execution Model

```yaml
Grid (Kernel Launch)
├── Block (0,0)
│   ├── Thread (0,0)
│   ├── Thread (0,1)
│   └── ...
├── Block (0,1)
│   └── Threads...
└── ...
```

#### Thread Hierarchy

| Level | Index Variables | Dimensions | Max Size |
|-------|----------------|------------|----------|
| **Thread** | `threadIdx.x/y/z` | 1D, 2D, 3D | 1024 threads/block |
| **Block** | `blockIdx.x/y/z` | 1D, 2D, 3D | Device specific (65535+) |
| **Grid** | - | 1D, 2D, 3D | Kernel-wide |

### Basic CUDA Program Structure

```cuda
// 1. Include headers
#include <cuda_runtime.h>
#include <stdio.h>

// 2. Define kernel (runs on GPU)
__global__ void vectorAdd(float *a, float *b, float *c, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        c[idx] = a[idx] + b[idx];
    }
}

// 3. Host code (runs on CPU)
int main() {
    int n = 1000000;
    size_t bytes = n * sizeof(float);
    
    // Allocate host memory
    float *h_a = (float*)malloc(bytes);
    float *h_b = (float*)malloc(bytes);
    float *h_c = (float*)malloc(bytes);
    
    // Initialize data
    for (int i = 0; i < n; i++) {
        h_a[i] = i;
        h_b[i] = i * 2;
    }
    
    // Allocate device memory
    float *d_a, *d_b, *d_c;
    cudaMalloc(&d_a, bytes);
    cudaMalloc(&d_b, bytes);
    cudaMalloc(&d_c, bytes);
    
    // Copy data to device
    cudaMemcpy(d_a, h_a, bytes, cudaMemcpyHostToDevice);
    cudaMemcpy(d_b, h_b, bytes, cudaMemcpyHostToDevice);
    
    // Launch kernel
    int threadsPerBlock = 256;
    int blocksPerGrid = (n + threadsPerBlock - 1) / threadsPerBlock;
    vectorAdd<<<blocksPerGrid, threadsPerBlock>>>(d_a, d_b, d_c, n);
    
    // Copy result back to host
    cudaMemcpy(h_c, d_c, bytes, cudaMemcpyDeviceToHost);
    
    // Cleanup
    cudaFree(d_a);
    cudaFree(d_b);
    cudaFree(d_c);
    free(h_a);
    free(h_b);
    free(h_c);
    
    return 0;
}
```

### CUDA Function Qualifiers

| Qualifier | Executes On | Callable From | Purpose |
|-----------|-------------|---------------|---------|
| `__global__` | Device (GPU) | Host (CPU) | Kernel function |
| `__device__` | Device | Device | GPU helper function |
| `__host__` | Host | Host | CPU function (default) |
| `__host__ __device__` | Both | Both | Compiled for both CPU/GPU |

### Memory Management Functions

```cuda
// Allocation
cudaMalloc(void **ptr, size_t size);
cudaMallocManaged(void **ptr, size_t size);  // Unified Memory

// Copying
cudaMemcpy(void *dst, void *src, size_t size, cudaMemcpyKind kind);
// cudaMemcpyKind: HostToDevice, DeviceToHost, DeviceToDevice, HostToHost

// Initialization
cudaMemset(void *ptr, int value, size_t count);

// Freeing
cudaFree(void *ptr);
```

### Kernel Launch Configuration

```cuda
// Syntax
kernel_name<<<gridDim, blockDim, sharedMemBytes, stream>>>(arguments);

// Examples
// 1D Grid and Block
myKernel<<<100, 256>>>(args);  // 100 blocks, 256 threads/block

// 2D Grid and Block
dim3 grid(16, 16);      // 16x16 blocks
dim3 block(32, 32);     // 32x32 threads per block
myKernel<<<grid, block>>>(args);

// With shared memory
myKernel<<<grid, block, 1024>>>(args);  // 1024 bytes shared memory
```

### Built-in Variables

```cuda
// Thread indices
threadIdx.x, threadIdx.y, threadIdx.z  // Thread ID within block
blockIdx.x, blockIdx.y, blockIdx.z     // Block ID within grid
blockDim.x, blockDim.y, blockDim.z     // Block dimensions
gridDim.x, gridDim.y, gridDim.z        // Grid dimensions

// Calculate global thread ID
int tid = blockIdx.x * blockDim.x + threadIdx.x;  // 1D
int tid = (blockIdx.y * gridDim.x + blockIdx.x) * blockDim.x + threadIdx.x;  // 2D
```

### Error Checking

```cuda
// Macro for error checking
#define CUDA_CHECK(call) \
    do { \
        cudaError_t err = call; \
        if (err != cudaSuccess) { \
            fprintf(stderr, "CUDA error in %s:%d: %s\n", \
                    __FILE__, __LINE__, cudaGetErrorString(err)); \
            exit(EXIT_FAILURE); \
        } \
    } while(0)

// Usage
CUDA_CHECK(cudaMalloc(&d_data, size));
CUDA_CHECK(cudaMemcpy(d_data, h_data, size, cudaMemcpyHostToDevice));

// Check kernel errors
myKernel<<<grid, block>>>(args);
CUDA_CHECK(cudaGetLastError());
CUDA_CHECK(cudaDeviceSynchronize());
```

### Compilation

```bash
# Basic compilation
nvcc program.cu -o program

# With optimization
nvcc -O3 program.cu -o program

# Specify architecture
nvcc -arch=sm_80 program.cu -o program  # Ampere
nvcc -arch=sm_86 program.cu -o program  # RTX 30 series

# Multiple architectures (PTX + Binary)
nvcc -gencode arch=compute_70,code=sm_70 \
     -gencode arch=compute_80,code=sm_80 \
     -gencode arch=compute_86,code=sm_86 \
     program.cu -o program

# With debugging
nvcc -g -G program.cu -o program  # -G disables optimizations for debugging

# Link with libraries
nvcc program.cu -o program -lcublas -lcudnn
```

### Compute Capability Targets

| Architecture | Compute Capability | nvcc Flag |
|--------------|-------------------|-----------|
| Pascal | 6.0, 6.1, 6.2 | `-arch=sm_60` |
| Volta | 7.0 | `-arch=sm_70` |
| Turing | 7.5 | `-arch=sm_75` |
| Ampere | 8.0, 8.6 | `-arch=sm_80`, `-arch=sm_86` |
| Ada | 8.9 | `-arch=sm_89` |
| Hopper | 9.0 | `-arch=sm_90` |

---

## 🆔 Profiling

### Why Profile?

Profiling helps identify:

- **Performance bottlenecks**: Where your code spends most time
- **Memory issues**: Inefficient memory access patterns
- **Occupancy problems**: Underutilized GPU resources
- **Kernel inefficiencies**: Suboptimal thread configurations

### NVIDIA Profiling Tools

| Tool | Purpose | Use Case | Interface |
|------|---------|----------|-----------|
| **Nsight Systems** | System-wide analysis | Find CPU/GPU bottlenecks, understand workflow | GUI + CLI |
| **Nsight Compute** | Kernel profiling | Optimize individual kernels, memory analysis | GUI + CLI |
| **nvprof** | Legacy profiler | Quick command-line profiling (deprecated) | CLI |
| **CUDA Events** | Code instrumentation | Measure specific code sections | API |

### Nsight Systems (System-Level Profiling)

#### Basic Usage

```bash
# Profile application
nsys profile --stats=true ./myprogram

# With specific output
nsys profile -o my_report ./myprogram

# Profile specific section (API calls)
nsys profile --trace=cuda,nvtx ./myprogram

# Advanced: Capture CPU + GPU
nsys profile --trace=cuda,nvtx,osrt,cublas ./myprogram
```

#### Key Metrics to Examine

- **Timeline**: Visualize CPU/GPU activity over time
- **CUDA API calls**: Memory transfers, kernel launches
- **Kernel execution**: Duration, concurrency
- **Memory transfers**: Size, direction, duration
- **CPU threads**: Host code execution

#### Interpretation

```bash
# Generate report
nsys stats my_report.nsys-rep

# Look for:
# - Large gaps between kernel launches (CPU overhead)
# - Memory transfer overhead (consider unified memory)
# - Low GPU utilization (need more parallelism)
# - Serialized kernel execution (use streams)
```

### Nsight Compute (Kernel-Level Profiling)

#### Basic Usage of Nsight Comptue

```bash
# Profile all kernels
ncu ./myprogram

# Profile specific kernel
ncu --kernel-name myKernel ./myprogram

# Full analysis with all metrics
ncu --set full ./myprogram

# Export report
ncu -o kernel_report --set full ./myprogram

# Open report in GUI
ncu-ui kernel_report.ncu-rep
```

#### Key Sections

##### 1. GPU Speed of Light (SOL)

```text
Compute (SM) Throughput:  45%  [Low utilization]
Memory Throughput:        85%  [Memory bound!]
```

- **SM Utilization < 60%**: Compute-bound or low occupancy
- **Memory Throughput > 80%**: Memory-bound (optimize memory access)

##### 2. Occupancy

```text
Theoretical Occupancy:  75%
Achieved Occupancy:     62%  [Can improve]
```

- **Low occupancy**: Adjust block size, reduce register/shared memory usage
- **Target**: 50-70% usually sufficient

##### 3. Memory Workload Analysis

```text
Global Memory Load Efficiency:   45%  [Poor coalescing]
Global Memory Store Efficiency:  80%  [Good]
L1/L2 Cache Hit Rate:            35%  [Low]
```

- **Low efficiency**: Improve memory access patterns
- **Low cache hit rate**: Consider data locality

##### 4. Warp Execution

```text
Branch Divergence:        High
Warp Execution Efficiency: 65%
```

- **High divergence**: Minimize conditional statements
- **Low efficiency**: Optimize thread utilization

#### Optimization Workflow

```bash
# 1. Identify bottleneck
ncu --set full ./myprogram

# 2. Focus on specific metric
ncu --metrics sm__throughput.avg.pct_of_peak_sustained_elapsed ./myprogram

# 3. Compare before/after
ncu --metrics gpu__time_duration.sum --csv ./myprogram > baseline.csv
# (make optimizations)
ncu --metrics gpu__time_duration.sum --csv ./myprogram > optimized.csv
```

### CUDA Events (Code Instrumentation)

#### Basic Timing

```cuda
#include <cuda_runtime.h>

// Create events
cudaEvent_t start, stop;
cudaEventCreate(&start);
cudaEventCreate(&stop);

// Record start time
cudaEventRecord(start);

// Launch kernel
myKernel<<<grid, block>>>(args);

// Record end time
cudaEventRecord(stop);

// Wait for completion
cudaEventSynchronize(stop);

// Calculate elapsed time
float milliseconds = 0;
cudaEventElapsedTime(&milliseconds, start, stop);
printf("Kernel time: %f ms\n", milliseconds);

// Cleanup
cudaEventDestroy(start);
cudaEventDestroy(stop);
```

#### Timing Multiple Kernels

```cuda
cudaEvent_t start, stop, middle;
cudaEventCreate(&start);
cudaEventCreate(&middle);
cudaEventCreate(&stop);

cudaEventRecord(start);
kernel1<<<grid, block>>>(args);
cudaEventRecord(middle);
kernel2<<<grid, block>>>(args);
cudaEventRecord(stop);

cudaEventSynchronize(stop);

float time1, time2;
cudaEventElapsedTime(&time1, start, middle);
cudaEventElapsedTime(&time2, middle, stop);

printf("Kernel 1: %f ms\n", time1);
printf("Kernel 2: %f ms\n", time2);
```

### NVTX (NVIDIA Tools Extension)

#### Marking Code Regions

```cuda
#include <nvToolsExt.h>

// Mark function/region
void myFunction() {
    nvtxRangePush("MyFunction");
    
    // Your code here
    kernel<<<grid, block>>>(args);
    
    nvtxRangePop();
}

// Color-coded markers
void processData() {
    // Green for memory transfer
    nvtxEventAttributes_t eventAttrib = {0};
    eventAttrib.version = NVTX_VERSION;
    eventAttrib.size = NVTX_EVENT_ATTRIB_STRUCT_SIZE;
    eventAttrib.colorType = NVTX_COLOR_ARGB;
    eventAttrib.color = 0xFF00FF00;  // Green
    eventAttrib.messageType = NVTX_MESSAGE_TYPE_ASCII;
    eventAttrib.message.ascii = "Memory Transfer";
    
    nvtxRangePushEx(&eventAttrib);
    cudaMemcpy(d_data, h_data, size, cudaMemcpyHostToDevice);
    nvtxRangePop();
    
    // Blue for computation
    eventAttrib.color = 0xFF0000FF;  // Blue
    eventAttrib.message.ascii = "Kernel Execution";
    nvtxRangePushEx(&eventAttrib);
    kernel<<<grid, block>>>(args);
    nvtxRangePop();
}

// Compile with NVTX
// nvcc -lnvToolsExt program.cu -o program
```

### Performance Metrics Glossary

| Metric | Description | Target |
|--------|-------------|--------|
| **Occupancy** | % of active warps vs max possible | 50-75% |
| **SM Efficiency** | % of time SMs are active | > 60% |
| **IPC** | Instructions per cycle | Architecture dependent |
| **Memory Throughput** | Data transferred per second | Match kernel needs |
| **Coalescing Efficiency** | % of coalesced memory accesses | > 80% |
| **Branch Efficiency** | % of non-divergent branches | > 90% |
| **Warp Execution Eff.** | % of active threads in warps | > 80% |

### Common Profiling Commands Reference

```bash
# Nsight Systems
nsys profile --stats=true -o report ./app
nsys profile --trace=cuda,nvtx,cublas -o report ./app
nsys stats report.nsys-rep

# Nsight Compute
ncu --set full -o report ./app
ncu --kernel-name "myKernel" --launch-skip 0 --launch-count 1 ./app
ncu --query-metrics  # List all available metrics
ncu --metrics sm__throughput.avg.pct_of_peak_sustained_elapsed ./app

# Legacy nvprof (deprecated but still useful)
nvprof ./app
nvprof --print-gpu-trace ./app
nvprof --metrics achieved_occupancy,gld_efficiency ./app
```

### Profiling Best Practices

1. **Profile production workloads**: Use realistic data sizes
2. **Profile multiple runs**: Ignore cold start overhead
3. **Focus on hotspots**: Optimize the 20% that takes 80% time
4. **Measure incrementally**: Profile after each optimization
5. **Use NVTX markers**: Label important code sections
6. **Check multiple metrics**: Don't optimize blindly
7. **Consider Amdahl's Law**: GPU optimization has limits if CPU is bottleneck

---

## 🎯 Performance Analysis

### Performance Optimization Strategy

```text
1. Profile → 2. Identify Bottleneck → 3. Optimize → 4. Measure → 5. Repeat
```

### Types of Bottlenecks

| Type | Symptoms | Solutions |
|------|----------|-----------|
| **Memory Bound** | High memory throughput, low SM utilization | Improve coalescing, use shared memory, optimize access patterns |
| **Compute Bound** | High SM utilization, low memory throughput | Increase ILP, reduce operations, use specialized cores |
| **Latency Bound** | Low occupancy, high latency | Increase parallelism, hide latency with more threads |
| **Bandwidth Bound** | PCIe saturation | Minimize transfers, use pinned memory, batch operations |

### Memory Performance

#### Memory Hierarchy

| Memory Type | Latency | Bandwidth | Scope | Size |
|-------------|---------|-----------|-------|------|
| **Registers** | 1 cycle | ~TB/s | Thread | 64KB/SM |
| **L1 Cache** | ~5 cycles | ~7 TB/s | SM | 128KB/SM |
| **Shared Memory** | ~30 cycles | ~15 TB/s | Block | 48-164KB/SM |
| **L2 Cache** | ~200 cycles | ~2 TB/s | Device | 6-60MB |
| **Global Memory** | ~400 cycles | 0.9-3 TB/s | Device | GB-192GB |
| **Host Memory** | ~100,000 cycles | 16-32 GB/s | System | System RAM |

#### Coalesced Memory Access

##### **Bad: Strided Access**

```cuda
// Each thread accesses memory with stride
__global__ void stridedAccess(float *data, int stride) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    float value = data[idx * stride];  // BAD: Non-coalesced
}
```

##### **Good: Coalesced Access**

```cuda
// Sequential access pattern
__global__ void coalescedAccess(float *data) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    float value = data[idx];  // GOOD: Coalesced
}
```

#### Memory Alignment

```cuda
// Align data structures to 128 bytes
struct __align__(16) MyData {  // 16-byte alignment
    float x, y, z, w;
};

// Use aligned memory allocation
float *d_data;
cudaMalloc(&d_data, size);  // Already aligned
```

### Occupancy Optimization

#### Theoretical Occupancy Calculation

```text
Occupancy = Active Warps / Maximum Possible Warps

Factors limiting occupancy:
- Threads per block
- Registers per thread
- Shared memory per block
- Maximum warps per SM
```

#### Occupancy Calculator

```cuda
// Check occupancy at runtime
int blockSize = 256;
int minGridSize, optimalBlockSize;
cudaOccupancyMaxPotentialBlockSize(&minGridSize, &optimalBlockSize, 
                                    myKernel, 0, 0);
printf("Suggested block size: %d\n", optimalBlockSize);

// Manual calculation
cudaDeviceProp prop;
cudaGetDeviceProperties(&prop, 0);
int maxThreadsPerSM = prop.maxThreadsPerMultiProcessor;
int maxBlocksPerSM = prop.maxBlocksPerMultiProcessor;
```

#### Tuning Block Size

```cuda
// Test different block sizes
int blockSizes[] = {64, 128, 256, 512, 1024};
float bestTime = FLT_MAX;
int bestBlockSize = 0;

for (int i = 0; i < 5; i++) {
    int blockSize = blockSizes[i];
    int gridSize = (n + blockSize - 1) / blockSize;
    
    cudaEvent_t start, stop;
    cudaEventCreate(&start);
    cudaEventCreate(&stop);
    
    cudaEventRecord(start);
    myKernel<<<gridSize, blockSize>>>(args);
    cudaEventRecord(stop);
    cudaEventSynchronize(stop);
    
    float ms;
    cudaEventElapsedTime(&ms, start, stop);
    
    if (ms < bestTime) {
        bestTime = ms;
        bestBlockSize = blockSize;
    }
}
printf("Optimal block size: %d (%.2f ms)\n", bestBlockSize, bestTime);
```

### Instruction-Level Parallelism (ILP)

#### Increase ILP with Unrolling

```cuda
// Low ILP
__global__ void lowILP(float *a, float *b, float *c, int n) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) {
        c[i] = a[i] + b[i];
    }
}

// Higher ILP - each thread processes 4 elements
__global__ void higherILP(float *a, float *b, float *c, int n) {
    int i = 4 * (blockIdx.x * blockDim.x + threadIdx.x);
    if (i + 3 < n) {
        c[i]   = a[i]   + b[i];
        c[i+1] = a[i+1] + b[i+1];
        c[i+2] = a[i+2] + b[i+2];
        c[i+3] = a[i+3] + b[i+3];
    }
}
```

### Branch Divergence Mitigation

#### Problem: Divergent Branches

```cuda
// BAD: Highly divergent
__global__ void divergentKernel(int *data, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        if (data[idx] % 2 == 0) {
            // Even threads do this
            data[idx] *= 2;
        } else {
            // Odd threads do this
            data[idx] *= 3;
        }
    }
}
```

#### Solution: Minimize Divergence

```cuda
// BETTER: Reduce divergence with arithmetic
__global__ void lessDiv ergentKernel(int *data, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        int factor = 2 + (data[idx] & 1);  // 2 for even, 3 for odd
        data[idx] *= factor;
    }
}

// BEST: Sort data to group similar branches
// Process even and odd numbers in separate kernel calls
```

### Shared Memory Optimization

#### Bank Conflicts

```cuda
// BAD: Bank conflict (32 threads access same bank)
__shared__ float sharedMem[32];
int tid = threadIdx.x;
float value = sharedMem[tid % 32];  // All threads hit same 32 banks

// GOOD: No bank conflict (sequential access)
__shared__ float sharedMem[256];
int tid = threadIdx.x;
float value = sharedMem[tid];  // Each thread accesses different bank
```

#### Padding to Avoid Conflicts

```cuda
// Without padding
__shared__ float sharedMem[32][32];  // May cause conflicts

// With padding
__shared__ float sharedMem[32][33];  // Extra column avoids conflicts
```

### Kernel Fusion

```cuda
// BAD: Multiple kernel launches
kernel1<<<grid, block>>>(d_a, d_b);
cudaDeviceSynchronize();
kernel2<<<grid, block>>>(d_b, d_c);
cudaDeviceSynchronize();

// GOOD: Fused kernel
__global__ void fusedKernel(float *a, float *c) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    // Do both operations
    float temp = kernel1_operation(a[idx]);
    c[idx] = kernel2_operation(temp);
}
fusedKernel<<<grid, block>>>(d_a, d_c);
```

### Asynchronous Execution and Streams

```cuda
// Create streams
cudaStream_t stream[4];
for (int i = 0; i < 4; i++) {
    cudaStreamCreate(&stream[i]);
}

// Overlap data transfer and computation
for (int i = 0; i < 4; i++) {
    cudaMemcpyAsync(d_data[i], h_data[i], size, 
                    cudaMemcpyHostToDevice, stream[i]);
    kernel<<<grid, block, 0, stream[i]>>>(d_data[i]);
    cudaMemcpyAsync(h_result[i], d_result[i], size,
                    cudaMemcpyDeviceToHost, stream[i]);
}

// Synchronize all streams
cudaDeviceSynchronize();
```

### Performance Checklist

- [ ] **Memory access is coalesced**
- [ ] **Occupancy is 50-75%**
- [ ] **Shared memory used for reused data**
- [ ] **Bank conflicts minimized**
- [ ] **Branch divergence reduced**
- [ ] **Register usage optimized**
- [ ] **Asynchronous operations used**
- [ ] **Multiple streams for overlapping**
- [ ] **Pinned memory for transfers**
- [ ] **Appropriate block size chosen**

---

---

## 🗃️ 2D Indexing

### Understanding 2D Thread Organization

#### Thread Indexing in 2D Grid

```cuda
// 2D grid and block configuration
dim3 blockDim(BLOCK_WIDTH, BLOCK_HEIGHT);  // e.g., (16, 16)
dim3 gridDim(GRID_WIDTH, GRID_HEIGHT);      // e.g., (32, 32)

kernel<<<gridDim, blockDim>>>(args);
```

#### Calculating 2D Global Index

```cuda
__global__ void kernel2D(float *data, int width, int height) {
    // Calculate 2D indices
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    // Boundary check
    if (col < width && row < height) {
        // Convert 2D to 1D index (row-major)
        int idx = row * width + col;
        
        // Access data
        data[idx] = someValue;
    }
}
```

### Memory Layout: Row-Major vs Column-Major

#### Row-Major Order (C/CUDA Default)

```text
Matrix[row][col]
[0,0] [0,1] [0,2]    →  Memory: [0,0][0,1][0,2][1,0][1,1][1,2]...
[1,0] [1,1] [1,2]
```

```cuda
// Row-major indexing
int idx = row * width + col;
```

#### Column-Major Order (Fortran)

```text
Matrix[row][col]
[0,0] [0,1] [0,2]    →  Memory: [0,0][1,0][2,0][0,1][1,1][2,1]...
[1,0] [1,1] [1,2]
```

```cuda
// Column-major indexing
int idx = col * height + row;
```

### Matrix Addition Example

```cuda
#define WIDTH 1024
#define HEIGHT 1024
#define BLOCK_SIZE 16

__global__ void matrixAdd(float *A, float *B, float *C, int width, int height) {
    // 2D thread coordinates
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    // Boundary check
    if (col < width && row < height) {
        int idx = row * width + col;
        C[idx] = A[idx] + B[idx];
    }
}

int main() {
    size_t bytes = WIDTH * HEIGHT * sizeof(float);
    
    // Allocate memory
    float *d_A, *d_B, *d_C;
    cudaMalloc(&d_A, bytes);
    cudaMalloc(&d_B, bytes);
    cudaMalloc(&d_C, bytes);
    
    // Define block and grid dimensions
    dim3 blockDim(BLOCK_SIZE, BLOCK_SIZE);
    dim3 gridDim((WIDTH + BLOCK_SIZE - 1) / BLOCK_SIZE,
                 (HEIGHT + BLOCK_SIZE - 1) / BLOCK_SIZE);
    
    // Launch kernel
    matrixAdd<<<gridDim, blockDim>>>(d_A, d_B, d_C, WIDTH, HEIGHT);
    
    cudaDeviceSynchronize();
    return 0;
}
```

### Matrix Transpose (Naive)

```cuda
__global__ void transposeNaive(float *input, float *output, 
                                int width, int height) {
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    if (col < width && row < height) {
        int idxIn = row * width + col;
        int idxOut = col * height + row;  // Transposed indices
        output[idxOut] = input[idxIn];
    }
}
```

### Matrix Transpose (Optimized with Shared Memory)

```cuda
#define TILE_DIM 32

__global__ void transposeOptimized(float *input, float *output,
                                    int width, int height) {
    // Shared memory tile (with padding to avoid bank conflicts)
    __shared__ float tile[TILE_DIM][TILE_DIM + 1];
    
    // Global position
    int x = blockIdx.x * TILE_DIM + threadIdx.x;
    int y = blockIdx.y * TILE_DIM + threadIdx.y;
    
    // Load tile from global memory (coalesced read)
    if (x < width && y < height) {
        int idxIn = y * width + x;
        tile[threadIdx.y][threadIdx.x] = input[idxIn];
    }
    
    __syncthreads();
    
    // Transposed global position
    x = blockIdx.y * TILE_DIM + threadIdx.x;
    y = blockIdx.x * TILE_DIM + threadIdx.y;
    
    // Store tile to global memory (coalesced write)
    if (x < height && y < width) {
        int idxOut = y * height + x;
        output[idxOut] = tile[threadIdx.x][threadIdx.y];
    }
}
```

### 2D Convolution Example

```cuda
#define FILTER_SIZE 5
#define FILTER_RADIUS 2

__global__ void convolution2D(float *input, float *output, float *filter,
                               int width, int height) {
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    if (col < width && row < height) {
        float sum = 0.0f;
        
        // Apply filter
        for (int fRow = -FILTER_RADIUS; fRow <= FILTER_RADIUS; fRow++) {
            for (int fCol = -FILTER_RADIUS; fCol <= FILTER_RADIUS; fCol++) {
                int imageRow = row + fRow;
                int imageCol = col + fCol;
                
                // Boundary check
                if (imageRow >= 0 && imageRow < height &&
                    imageCol >= 0 && imageCol < width) {
                    
                    int imageIdx = imageRow * width + imageCol;
                    int filterIdx = (fRow + FILTER_RADIUS) * FILTER_SIZE +
                                    (fCol + FILTER_RADIUS);
                    
                    sum += input[imageIdx] * filter[filterIdx];
                }
            }
        }
        
        int idx = row * width + col;
        output[idx] = sum;
    }
}
```

### Image Processing: Grayscale Conversion

```cuda
__global__ void rgbToGrayscale(unsigned char *input, unsigned char *output,
                                int width, int height) {
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    if (col < width && row < height) {
        int idx = row * width + col;
        int rgbIdx = idx * 3;  // RGB has 3 channels
        
        unsigned char r = input[rgbIdx];
        unsigned char g = input[rgbIdx + 1];
        unsigned char b = input[rgbIdx + 2];
        
        // Weighted average for perceptual accuracy
        output[idx] = (unsigned char)(0.299f * r + 0.587f * g + 0.114f * b);
    }
}

// Launch
dim3 blockDim(16, 16);
dim3 gridDim((width + 15) / 16, (height + 15) / 16);
rgbToGrayscale<<<gridDim, blockDim>>>(d_input, d_output, width, height);
```

### 3D Indexing Extension

```cuda
__global__ void kernel3D(float *data, int width, int height, int depth) {
    // 3D thread coordinates
    int x = blockIdx.x * blockDim.x + threadIdx.x;
    int y = blockIdx.y * blockDim.y + threadIdx.y;
    int z = blockIdx.z * blockDim.z + threadIdx.z;
    
    if (x < width && y < height && z < depth) {
        // 3D to 1D index
        int idx = z * (width * height) + y * width + x;
        
        // Process data
        data[idx] = someValue;
    }
}

// Launch 3D kernel
dim3 blockDim(8, 8, 8);
dim3 gridDim((width + 7) / 8, (height + 7) / 8, (depth + 7) / 8);
kernel3D<<<gridDim, blockDim>>>(d_data, width, height, depth);
```

### Block and Grid Size Calculations

```cuda
// Helper function for grid size calculation
dim3 calculateGridSize(int width, int height, int blockWidth, int blockHeight) {
    int gridWidth = (width + blockWidth - 1) / blockWidth;
    int gridHeight = (height + blockHeight - 1) / blockHeight;
    return dim3(gridWidth, gridHeight);
}

// Usage
dim3 blockDim(16, 16);
dim3 gridDim = calculateGridSize(imageWidth, imageHeight, 16, 16);
myKernel<<<gridDim, blockDim>>>(args);
```

### Coalescing in 2D Access Patterns

```cuda
// GOOD: Coalesced - threads in same warp access adjacent columns
__global__ void goodCoalescing(float *data, int width, int height) {
    int col = blockIdx.x * blockDim.x + threadIdx.x;  // Varies within warp
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    if (col < width && row < height) {
        int idx = row * width + col;  // Adjacent threads → adjacent memory
        float val = data[idx];
    }
}

// BAD: Non-coalesced - threads access different rows
__global__ void badCoalescing(float *data, int width, int height) {
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    
    if (col < width && row < height) {
        int idx = col * height + row;  // Strided access, poor coalescing
        float val = data[idx];
    }
}
```

---

## 🧠 Shared Memory and Warp Divergence

### Shared Memory Overview

**Shared memory** is a user-managed cache that is:

- **Fast**: ~100x faster than global memory
- **Limited**: 48KB to 164KB per SM (architecture-dependent)
- **Scope**: Accessible by all threads in a block
- **Lifetime**: Exists for duration of block execution

### Shared Memory Declaration

```cuda
// Static allocation (size known at compile time)
__global__ void kernel() {
    __shared__ float sharedData[256];
    // Use sharedData...
}

// Dynamic allocation (size determined at launch)
__global__ void kernelDynamic() {
    extern __shared__ float sharedData[];  // Size specified at launch
    // Use sharedData...
}

// Launch with dynamic shared memory
kernel<<<grid, block, sharedMemSize>>>(args);
//                     ^^^^^^^^^^^^^^ bytes of shared memory
```

### Shared Memory Banks

- **32 banks** (on most architectures)
- **4-byte wide banks**
- **Bank conflict**: Multiple threads in a warp access the same bank

```text
Bank 0:  [0][32][64]...
Bank 1:  [1][33][65]...
Bank 2:  [2][34][66]...
...
Bank 31: [31][63][95]...
```

#### No Conflict (Sequential Access)

```cuda
__shared__ float sharedMem[256];
int tid = threadIdx.x;
float value = sharedMem[tid];  // Each thread → different bank
```

#### Bank Conflict Example

```cuda
__shared__ float sharedMem[256];
int tid = threadIdx.x;
// Multiple threads access same bank
float value = sharedMem[(tid * 2) % 32];  // BAD: 2-way conflict
```

#### Avoiding Bank Conflicts with Padding

```cuda
// Without padding: [32][32] causes conflicts on column access
__shared__ float tile[32][32];

// With padding: [32][33] shifts elements to different banks
__shared__ float tile[32][33];

// Access pattern
int row = threadIdx.y;
int col = threadIdx.x;
float val = tile[row][col];  // No conflict with padding
```

### Matrix Multiplication with Shared Memory

```cuda
#define TILE_WIDTH 16

__global__ void matrixMulShared(float *A, float *B, float *C,
                                 int width) {
    // Shared memory for tiles
    __shared__ float tileA[TILE_WIDTH][TILE_WIDTH];
    __shared__ float tileB[TILE_WIDTH][TILE_WIDTH];
    
    int row = blockIdx.y * TILE_WIDTH + threadIdx.y;
    int col = blockIdx.x * TILE_WIDTH + threadIdx.x;
    
    float sum = 0.0f;
    
    // Loop over tiles
    for (int t = 0; t < (width + TILE_WIDTH - 1) / TILE_WIDTH; t++) {
        // Load tiles into shared memory
        if (row < width && t * TILE_WIDTH + threadIdx.x < width) {
            tileA[threadIdx.y][threadIdx.x] = 
                A[row * width + t * TILE_WIDTH + threadIdx.x];
        } else {
            tileA[threadIdx.y][threadIdx.x] = 0.0f;
        }
        
        if (col < width && t * TILE_WIDTH + threadIdx.y < width) {
            tileB[threadIdx.y][threadIdx.x] = 
                B[(t * TILE_WIDTH + threadIdx.y) * width + col];
        } else {
            tileB[threadIdx.y][threadIdx.x] = 0.0f;
        }
        
        __syncthreads();  // Wait for all threads to load
        
        // Compute partial product
        for (int k = 0; k < TILE_WIDTH; k++) {
            sum += tileA[threadIdx.y][k] * tileB[k][threadIdx.x];
        }
        
        __syncthreads();  // Wait before loading next tile
    }
    
    // Write result
    if (row < width && col < width) {
        C[row * width + col] = sum;
    }
}
```

### Thread Synchronization

```cuda
__syncthreads();  // Barrier: all threads in block must reach this point
```

**Important Rules:**

- Only synchronizes threads within the **same block**
- Must be called by **all threads** in the block (or none in divergent paths)
- Use before/after shared memory operations

#### Common Mistake: Divergent Sync

```cuda
// BAD: Not all threads reach __syncthreads()
if (threadIdx.x < 16) {
    __syncthreads();  // WRONG: Deadlock!
}

// GOOD: All threads participate
if (threadIdx.x < 16) {
    // Do work
}
__syncthreads();  // All threads reach here
```

### Warp Divergence

**Warp**: Group of 32 threads that execute in lockstep (SIMT - Single Instruction, Multiple Thread)

#### What Causes Divergence?

```cuda
// Divergent branch
if (threadIdx.x % 2 == 0) {
    // Even threads execute path A
    doWorkA();
} else {
    // Odd threads execute path B
    doWorkB();
}
// Both paths execute serially (performance loss)
```

#### Divergence Example

```text
Warp with 32 threads:
Thread 0-15: Take branch A → Execute A
Thread 16-31: Idle (wait)
Thread 0-15: Idle (wait)
Thread 16-31: Take branch B → Execute B
Result: 2x execution time
```

### Minimizing Warp Divergence

#### Technique 1: Warp-Aligned Conditions

```cuda
// BAD: Divergence within warp
if (threadIdx.x % 2 == 0) {
    // Work
}

// BETTER: Whole warps take same path
if (threadIdx.x < 64) {  // First 2 warps
    // Work
}
```

#### Technique 2: Avoid Data-Dependent Branches

```cuda
// BAD: Data-dependent branch
__global__ void divergentKernel(int *data) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (data[idx] > 0) {
        // Positive processing
    } else {
        // Negative processing
    }
}

// BETTER: Use arithmetic instead of branches
__global__ void branchlessKernel(int *data) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    int sign = (data[idx] > 0) ? 1 : -1;
    int result = data[idx] * sign * computeValue();
}
```

#### Technique 3: Sort Data

```cuda
// Pre-process: Sort data so similar items are grouped
// This reduces divergence when processing

// Process positive numbers
processPositive<<<grid, block>>>(positiveData, countPositive);

// Process negative numbers
processNegative<<<grid, block>>>(negativeData, countNegative);
```

### Warp-Level Primitives

#### Warp Shuffle

```cuda
__global__ void warpShuffleExample() {
    int value = threadIdx.x;
    
    // Get value from thread 0 in warp
    int broadcast = __shfl_sync(0xffffffff, value, 0);
    
    // Get value from next thread (circular)
    int neighbor = __shfl_sync(0xffffffff, value, (threadIdx.x + 1) % 32);
    
    // Butterfly exchange
    int partner = __shfl_xor_sync(0xffffffff, value, 1);
}
```

#### Warp Vote Functions

```cuda
__global__ void warpVoteExample(int *data) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    int value = data[idx];
    
    // Check if all threads in warp have value > 0
    int all_positive = __all_sync(0xffffffff, value > 0);
    
    // Check if any thread in warp has value > 100
    int any_large = __any_sync(0xffffffff, value > 100);
    
    // Count threads in warp where value is even
    int even_count = __popc(__ballot_sync(0xffffffff, value % 2 == 0));
}
```

### Shared Memory Use Cases

| Use Case | Benefit |
|----------|---------|
| **Tile-based algorithms** | Reuse data loaded from global memory |
| **Reduction operations** | Fast inter-thread communication |
| **Transpose operations** | Coalesce non-coalesced accesses |
| **Histogram** | Fast atomic operations |
| **Prefix sum (scan)** | Parallel cumulative computation |

### Performance Comparison

```cuda
// Global memory only
__global__ void withoutShared(float *input, float *output, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        float sum = 0;
        for (int i = -2; i <= 2; i++) {
            if (idx + i >= 0 && idx + i < n) {
                sum += input[idx + i];  // 5 global memory reads per thread
            }
        }
        output[idx] = sum / 5.0f;
    }
}

// With shared memory
__global__ void withShared(float *input, float *output, int n) {
    __shared__ float shared[256 + 4];  // Block size + halo
    
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    int localIdx = threadIdx.x + 2;
    
    // Load to shared memory (coalesced)
    if (idx < n) shared[localIdx] = input[idx];
    if (threadIdx.x < 2 && idx >= 2) shared[threadIdx.x] = input[idx - 2];
    if (threadIdx.x < 2 && idx + blockDim.x < n) 
        shared[localIdx + blockDim.x] = input[idx + blockDim.x];
    
    __syncthreads();
    
    if (idx < n) {
        float sum = 0;
        for (int i = -2; i <= 2; i++) {
            sum += shared[localIdx + i];  // Fast shared memory reads
        }
        output[idx] = sum / 5.0f;
    }
}
// Speedup: ~5-10x faster with shared memory
```

---

## 🐞 Debugging Tools

### CUDA Debugging Strategies

| Tool | Purpose | When to Use |
|------|---------|-------------|
| **printf()** | Simple output debugging | Quick checks, small data |
| **cuda-gdb** | Command-line debugger | Complex bugs, batch debugging |
| **Nsight VSCode** | IDE debugging | Interactive debugging, visualization |
| **compute-sanitizer** | Memory checker | Memory errors, race conditions |
| **CUDA-MEMCHECK** | Legacy memory checker | Older CUDA versions |

### Printf Debugging

```cuda
__global__ void debugKernel(float *data, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    
    // Print from first thread only
    if (idx == 0) {
        printf("Kernel launched with %d threads\n", n);
    }
    
    // Conditional debugging
    if (idx < n && data[idx] < 0) {
        printf("Thread %d: Negative value %.2f detected\n", idx, data[idx]);
    }
    
    // Debug specific thread
    if (blockIdx.x == 0 && threadIdx.x == 0) {
        printf("First thread in first block\n");
    }
}

// Flush printf buffer
cudaDeviceSynchronize();
```

**Limitations:**

- Output is buffered (may not appear immediately)
- Can slow down execution significantly
- Limited buffer size (~1MB)

### cuda-gdb (Command-Line Debugger)

#### Compilation for Debugging

```bash
# Compile with debug symbols and no optimization
nvcc -g -G -O0 program.cu -o program_debug

# -g: Host debug symbols
# -G: Device debug symbols
# -O0: Disable optimization
```

#### Basic Commands

```bash
# Start debugger
cuda-gdb ./program

# Set breakpoints
(cuda-gdb) break myKernel                    # Break at kernel
(cuda-gdb) break program.cu:42               # Break at line
(cuda-gdb) break myKernel if threadIdx.x==5  # Conditional breakpoint

# Run program
(cuda-gdb) run
(cuda-gdb) run arg1 arg2  # With arguments

# Execution control
(cuda-gdb) continue  # Resume execution
(cuda-gdb) step      # Step into
(cuda-gdb) next      # Step over
(cuda-gdb) finish    # Step out

# Inspect threads
(cuda-gdb) info cuda threads           # All CUDA threads
(cuda-gdb) cuda thread 0               # Switch to thread 0
(cuda-gdb) cuda block 1 thread 5       # Switch to specific thread
(cuda-gdb) cuda kernel block thread    # Show current focus

# Inspect data
(cuda-gdb) print variable
(cuda-gdb) print threadIdx.x
(cuda-gdb) print shared[threadIdx.x]

# Watch variables
(cuda-gdb) watch data[100]  # Break when value changes

# Backtrace
(cuda-gdb) backtrace
(cuda-gdb) bt

# Quit
(cuda-gdb) quit
```

#### Advanced cuda-gdb

```bash
# Debug specific GPU
(cuda-gdb) set cuda device 1

# Filter threads
(cuda-gdb) cuda thread (blockIdx.x == 0 && threadIdx.x < 32)

# Print array elements
(cuda-gdb) print data[0]@10  # Print first 10 elements

# Examine memory
(cuda-gdb) x/10fx data  # 10 floats in hex

# Show kernel info
(cuda-gdb) info cuda kernels
(cuda-gdb) info cuda blocks
(cuda-gdb) info cuda warps
```

### Compute Sanitizer

**Compute Sanitizer** detects:

- Memory access errors (out-of-bounds, uninitialized)
- Race conditions
- Memory leaks
- Shared memory data races
- Synchronization errors

#### Basic Usage of Sanitizer

```bash
# Run with all checks
compute-sanitizer ./program

# Memcheck tool (default)
compute-sanitizer --tool memcheck ./program

# Race detection
compute-sanitizer --tool racecheck ./program

# Initialize check
compute-sanitizer --tool initcheck ./program

# Synchronization check
compute-sanitizer --tool synccheck ./program
```

### Error Checking Macro

```cuda
#define CUDA_CHECK_ERROR() \
    do { \
        cudaError_t err = cudaGetLastError(); \
        if (err != cudaSuccess) { \
            fprintf(stderr, "CUDA Error: %s at %s:%d\n", \
                    cudaGetErrorString(err), __FILE__, __LINE__); \
            exit(EXIT_FAILURE); \
        } \
    } while (0)

// Usage after kernel launch
myKernel<<<grid, block>>>(args);
CUDA_CHECK_ERROR();
cudaDeviceSynchronize();
CUDA_CHECK_ERROR();
```

---

## 🏹 Vector Reduction

### What is Reduction?

**Reduction**: Combining array elements into a single value using an associative operator.

**Examples:**

- Sum: `result = a[0] + a[1] + ... + a[n-1]`
- Max: `result = max(a[0], a[1], ..., a[n-1])`
- Product: `result = a[0] * a[1] * ... * a[n-1]`

### Parallel Reduction Strategy

```text
Step 1: [1,2,3,4,5,6,7,8]
         ↓ ↓ ↓ ↓
Step 2: [3,  7,  11, 15]
         ↓     ↓
Step 3: [10,     26]
           ↓
Step 4:   [36]

Complexity: O(log n) steps with n/2 threads
```

### Optimized Shared Memory Reduction

```cuda
__global__ void reduceShared(float *input, float *output, int n) {
    extern __shared__ float sharedData[];
    
    int tid = threadIdx.x;
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    
    // Load data into shared memory
    sharedData[tid] = (idx < n) ? input[idx] : 0.0f;
    __syncthreads();
    
    // Reduction in shared memory
    for (int stride = blockDim.x / 2; stride > 0; stride >>= 1) {
        if (tid < stride) {
            sharedData[tid] += sharedData[tid + stride];
        }
        __syncthreads();
    }
    
    // Write result for this block
    if (tid == 0) {
        output[blockIdx.x] = sharedData[0];
    }
}

// Launch
int threadsPerBlock = 256;
int blocksPerGrid = (n + threadsPerBlock - 1) / threadsPerBlock;
reduceShared<<<blocksPerGrid, threadsPerBlock, threadsPerBlock * sizeof(float)>>>(d_input, d_output, n);
```

### Warp-Level Reduction (Shuffle)

```cuda
__inline__ __device__
float warpReduce(float val) {
    for (int offset = 16; offset > 0; offset /= 2) {
        val += __shfl_down_sync(0xffffffff, val, offset);
    }
    return val;
}

__global__ void reduceWarpShuffle(float *input, float *output, int n) {
    int tid = threadIdx.x;
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    
    float sum = (idx < n) ? input[idx] : 0.0f;
    
    // Warp-level reduction (no shared memory needed!)
    sum = warpReduce(sum);
    
    // First thread in each warp writes to shared memory
    __shared__ float warpSums[32];  // Max 32 warps per block
    int lane = threadIdx.x % 32;
    int warpId = threadIdx.x / 32;
    
    if (lane == 0) {
        warpSums[warpId] = sum;
    }
    __syncthreads();
    
    // Final reduction by first warp
    if (warpId == 0) {
        sum = (tid < blockDim.x / 32) ? warpSums[lane] : 0.0f;
        sum = warpReduce(sum);
        
        if (tid == 0) {
            output[blockIdx.x] = sum;
        }
    }
}
```

---

## 📏 Roofline Model

### What is the Roofline Model?

The **Roofline Model** is a visual performance model that helps understand:

- **Performance bounds** of your kernel
- Whether code is **compute-bound** or **memory-bound**
- **Optimization opportunities**

### Key Concepts about Roofline

#### Arithmetic Intensity (AI)

```text
Arithmetic Intensity = FLOPs / Bytes Transferred

AI = Operations / Data Movement
```

**Examples:**

- `y[i] = a * x[i] + y[i]` (SAXPY): AI = 2 FLOPs / 8 bytes = 0.25 FLOP/byte
- Matrix multiplication: AI ≈ O(N) FLOP/byte (high)
- Simple addition: AI ≈ 0.125 FLOP/byte (low)

#### Peak Performance

```text
Peak FLOPs = Max theoretical compute throughput
```

Example (RTX 3090):

- FP32: 35.6 TFLOPs
- FP16: 71.2 TFLOPs (with Tensor Cores: 285 TFLOPs)

#### Peak Memory Bandwidth

```text
Peak Bandwidth = Max memory throughput (GB/s)
```

Example (RTX 3090):

- Memory Bandwidth: 936 GB/s

### Roofline Formula

```text
Attainable Performance = min(Peak Compute, Peak Bandwidth × Arithmetic Intensity)
```

### Ridge Point

```text
Ridge Point = Peak Compute / Peak Bandwidth

If AI < Ridge Point: Memory Bound
If AI > Ridge Point: Compute Bound
```

**Example (A100):**

- Peak FP32: 19.5 TFLOPs = 19,500 GFLOPs
- Peak Bandwidth: 2 TB/s = 2000 GB/s
- Ridge Point = 19,500 / 2000 = 9.75 FLOPs/Byte

### Optimization Based on Roofline

**If Memory Bound (AI < Ridge Point):**

- ✅ Use shared memory to reduce global memory access
- ✅ Improve memory coalescing
- ✅ Increase data reuse (tiling)
- ✅ Use texture memory for read-only data
- ✅ Compress data if possible

**If Compute Bound (AI > Ridge Point):**

- ✅ Use specialized cores (Tensor Cores for FP16/INT8)
- ✅ Increase instruction-level parallelism
- ✅ Reduce arithmetic operations
- ✅ Use FMA (fused multiply-add) instructions
- ✅ Consider lower precision (FP16 instead of FP32)

---
