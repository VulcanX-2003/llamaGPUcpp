# llama.cpp GPU Inference in C++

This repository demonstrates how to run [llama.cpp](https://github.com/ggerganov/llama.cpp) inference from C++ with pure GPU acceleration. The example is taken from the original repository.


## Getting Started

1. **Clone this repository**
2. **Clone [llama.cpp](https://github.com/ggerganov/llama.cpp) into the `external` directory**
3. **Build the project** (see instructions below)
4. **Run inference using your GPU**

## Requirements

- C++17 or later
- CUDA-enabled GPU
- [llama.cpp](https://github.com/ggerganov/llama.cpp) with GPU support
- Save ur gguf files to a models directory

## Build Instructions

```bash
git clone https://github.com/VulcanX-2003/llamaGPUcpp.git
cd ZenoTest2
mkdir external
cd external
git clone --depth=1 https://github.com/ggerganov/llama.cpp
cd ..
cmake -B build -DLLAMA_CUDA=on #for other builds check the main llama.cpp repository
cmake --build build -- -j$(nproc)
```

## Usage
./build/my_llama_chat -m/models/yourmodel.gguf [-ngl gpu_layers]

