# Installation Guide

This project uses the conda environment setup method as discussed in the [official FAISS installation guide](https://github.com/facebookresearch/faiss/blob/main/INSTALL.md).

We will install the `faiss-gpu-cuvs` package to leverage GPU acceleration and get the optimized version.

## Prerequisites

Ensure Miniconda is installed or set up as described below. This guide is tested on Ubuntu 22.04 with Python 3.12, so we are using the Python 3.12 compatible variant.

### Install Miniconda

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-py312_25.7.0-2-Linux-x86_64.sh
bash ~/Miniconda3-latest-Linux-x86_64.sh
source ~/.bashrc
```

## Environment Setup

Create a conda environment with the required dependencies:

```bash
conda create -n faiss-dev -y \
    -c pytorch -c nvidia -c rapidsai -c conda-forge \
    python=3.11 \
    libnvjitlink \
    faiss-gpu-cuvs=1.12.0
```

## C++ Development Setup

For C++ development with FAISS GPU support, install additional CUDA development packages:

```bash
conda install -c nvidia cuda-nvcc cuda-cudart-dev libcudacxx
```

**Note:** The `faiss-gpu-cuvs` package uses NVIDIA's cuVS libraries instead of traditional `libfaiss_gpu` for GPU acceleration.

## Compilation and Runtime

### Compile C++ code (with RMM support):
```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -Wl,-rpath,$CONDA_PREFIX/lib \
    -DLIBCUDACXX_ENABLE_EXPERIMENTAL_MEMORY_RESOURCE \
    -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs -lrmm
```

The `-Wl,-rpath,$CONDA_PREFIX/lib` flag embeds the library path, eliminating the need for runtime library path configuration.

### Alternative compilation (requires runtime setup):
```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs -lrmm
sudo ldconfig $CONDA_PREFIX/lib
```

### RMM Header Usage

When using RMM headers in your C++ code, use the correct paths:

```cpp
#include <rmm/mr/device/device_memory_resource.hpp>
#include <rmm/mr/device/per_device_resource.hpp>
```

**Note:** RMM headers are located in the `device/` subdirectory, not directly in `mr/`.

## Next Steps

After creating the environment, activate it with:

```bash
conda activate faiss-dev
```