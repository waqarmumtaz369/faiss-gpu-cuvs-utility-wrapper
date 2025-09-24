# faiss-gpu-cuvs-utility-wrapper
This is a standalone module that stores  the vector data and performs a similarity search. A C++ API will be used with GPU cuVS (CUDA Vector Similarity) support. 

## Troubleshooting

### Common Compilation Issues

#### 1. Missing CUDA Headers
**Error:** `fatal error: crt/host_defines.h: No such file or directory`

**Solution:** Install CUDA development packages:
```bash
conda install -c nvidia cuda-nvcc cuda-cudart-dev
```

#### 2. Missing GPU Libraries
**Error:** `/usr/bin/ld: cannot find -lfaiss_gpu: No such file or directory`

**Solution:** Use cuVS libraries instead of traditional faiss_gpu:
```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs
```

#### 3. Runtime Library Loading
**Error:** `./faiss-gpu: error while loading shared libraries: libfaiss.so: cannot open shared object file`

**Solution:** Use rpath during compilation (recommended):
```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -Wl,-rpath,$CONDA_PREFIX/lib -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs
```

Or configure system library path:
```bash
sudo ldconfig $CONDA_PREFIX/lib
```

#### 4. RMM Compilation Errors
**Error:** `RMM requires LIBCUDACXX_ENABLE_EXPERIMENTAL_MEMORY_RESOURCE to be defined`

**Solution:** Add required compiler flags:
```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -Wl,-rpath,$CONDA_PREFIX/lib \
    -DLIBCUDACXX_ENABLE_EXPERIMENTAL_MEMORY_RESOURCE \
    -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs -lrmm
```

#### 5. CUDA Namespace Issues
**Error:** `'cuda' has not been declared` or `'cuda' does not name a type`

**Solution:** Install additional CUDA C++ packages:
```bash
conda install -c nvidia libcudacxx
```

#### 6. RMM Library Linking Errors
**Error:** `undefined reference to symbol '_ZN3rmm16cuda_stream_viewC1EP11CUstream_st'` or `DSO missing from command line`

**Solution:** Add RMM library to the linking command:
```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -Wl,-rpath,$CONDA_PREFIX/lib \
    -DLIBCUDACXX_ENABLE_EXPERIMENTAL_MEMORY_RESOURCE \
    -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs -lrmm
```

#### 7. RMM Header Path Issues
**Error:** `fatal error: rmm/mr/per_device_resource.hpp: No such file or directory`

**Solution:** Use the correct header path. RMM headers are located in the `device/` subdirectory:
```cpp
#include <rmm/mr/device/device_memory_resource.hpp>
#include <rmm/mr/device/per_device_resource.hpp>
```

### Complete Working Compilation Command

For the most reliable compilation with all required flags:

```bash
g++ -I$CONDA_PREFIX/include -L$CONDA_PREFIX/lib -Wl,-rpath,$CONDA_PREFIX/lib \
    -DLIBCUDACXX_ENABLE_EXPERIMENTAL_MEMORY_RESOURCE \
    -o faiss-gpu faiss-gpu.cpp -lfaiss -lcuvs -lrmm
```

### Required Conda Packages

Ensure you have all necessary packages installed:

```bash
conda install -c nvidia cuda-nvcc cuda-cudart-dev libcudacxx
```

### RMM Header Usage

When using RMM headers in your C++ code, use the correct paths:

```cpp
#include <rmm/mr/device/device_memory_resource.hpp>
#include <rmm/mr/device/per_device_resource.hpp>
```

**Note:** RMM headers are located in the `device/` subdirectory, not directly in `mr/`.