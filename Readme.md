# llama.cpp GPU Inference in C++

This repository demonstrates how to run [llama.cpp](https://github.com/ggerganov/llama.cpp) inference from C++ with optional GPU acceleration.

## Getting Started

1. Clone this repository.
2. Put or clone the upstream `llama.cpp` into the `external` directory.
3. Build the project (instructions below).
4. Run inference using your GPU or CPU.

## Requirements

- C++17 toolchain (compiler & build tools)
- CMake >= 3.18
- Git
- CUDA Toolkit (only if building with GPU support)
- Place GGUF model files in `models/`

## Directory Structure

```
llamaGPUcpp/
├── external/
│   └── llama.cpp/         # cloned llama.cpp repository
├── models/
│   └── yourmodel.gguf     # put GGUF model files here
├── build/                 # build output (after compilation)
├── Readme.md
├── CMakeLists.txt
└── src/main.cpp
```

## Build Instructions

Notes:
- This repo uses the CMake option `GGML_CUDA` to enable CUDA. Use `-DGGML_CUDA=ON` to enable GPU builds (default in examples below).
- If you prefer submodules: `git submodule update --init --recursive`.

Common initial steps (works on all OSes):
```bash
git clone https://github.com/VulcanX-2003/llamaGPUcpp.git
cd llamaGPUcpp
mkdir -p external
cd external
git clone --depth=1 https://github.com/ggerganov/llama.cpp
cd ..
```

Linux (example, Ubuntu / WSL)
```bash
# Install prerequisites (example)
sudo apt update
sudo apt install build-essential cmake git

# Configure & build (CPU or GPU)
cmake -S . -B build -DGGML_CUDA=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build -j$(nproc)
```
If CUDA cannot be found or you want CPU-only:
```bash
cmake -S . -B build -DGGML_CUDA=OFF -DCMAKE_BUILD_TYPE=Release
cmake --build build -j$(nproc)
```

Windows (PowerShell, MSVC)
Prereqs: Visual Studio (Desktop dev. for C++), CMake, optional CUDA Toolkit.
```powershell
git clone https://github.com/VulcanX-2003/llamaGPUcpp.git
cd llamaGPUcpp
mkdir external; cd external
git clone --depth=1 https://github.com/ggerganov/llama.cpp
cd ..

# Configure (select generator/arch with -A)
cmake -S . -B build -A x64 -DGGML_CUDA=ON -DCMAKE_BUILD_TYPE=Release

# Build (multi-proc)
cmake --build build --config Release -- /m
```
For CPU-only set `-DGGML_CUDA=OFF`. If using Ninja, pass `-G Ninja` and omit `-A`.

macOS (Intel / Apple Silicon)
Note: CUDA is not supported on Apple Silicon — use CPU builds or upstream Metal/accelerations.
```bash
# Install prerequisites: Xcode CLT, cmake, git (Homebrew example)
brew install cmake git

# Configure & build (CPU)
cmake -S . -B build -DGGML_CUDA=OFF -DCMAKE_BUILD_TYPE=Release
cmake --build build -j$(sysctl -n hw.ncpu)
```

## Run

Example:
```bash
# Linux / macOS
./build/chatTerm -m models/yourmodel.gguf -c 2048 -ngl 32

# Windows (PowerShell)
.\build\chatTerm.exe -m models\yourmodel.gguf -c 2048 -ngl 32
```
Options depend on `src/main.cpp`. Adjust `-m` (model path), `-c` (context length), and `-ngl` (GPU layers) as needed.

## Troubleshooting

- CMake cannot find CUDA: ensure CUDA Toolkit is installed and environment variables (PATH, CUDA_PATH) are set. On Windows, use a matching Visual Studio toolset and CUDA version.
- Generator / architecture mismatch on Windows: ensure `-A x64` matches your toolchain and Visual Studio target.
- If build fails, inspect `build/CMakeFiles/CMakeError.log` and CMake / compiler output.
- For advanced flags or backend options, refer to upstream llama.cpp docs.

## VS Code

Recommended extension: ms-vscode.cmake-tools for configuring and building inside VS Code.
