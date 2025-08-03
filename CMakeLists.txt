cmake_minimum_required(VERSION 3.18)
project(my_llama_chat LANGUAGES CXX CUDA)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(GGML_CUDA "Enable CUDA backend for ggml" ON)
set(BUILD_SHARED_LIBS OFF)  # 🔒 Use static lib

add_subdirectory(external/llama.cpp)

add_executable(my_llama_chat src/main.cpp)

target_include_directories(my_llama_chat PRIVATE
    external/llama.cpp
    external/llama.cpp/common
)

target_link_libraries(my_llama_chat PRIVATE llama)
