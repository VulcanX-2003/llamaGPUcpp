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

## Directory Structure
```markdown

llamaGPUcpp/
├── external/
│   └── llama.cpp/         # cloned llama.cpp repository
├── models/
│   └── yourmodel.gguf     # place your GGUF model files here
├── build/                 # build output (after compilation)
├── Readme.md
├── CMakeLists.txt         
└── src/main.cpp           # chat code

```


## Build Instructions

```bash
git clone https://github.com/VulcanX-2003/llamaGPUcpp.git
cd llmaGPUcpp
mkdir external
cd external
git clone --depth=1 https://github.com/ggerganov/llama.cpp
cd ..
cmake -B build -DLLAMA_CUDA=on #for other builds check the main llama.cpp repository
cmake --build build -- -j$(nproc)
```

## Usage
./build/my_llama_chat -m/models/yourmodel.gguf [-ngl gpu_layers]

