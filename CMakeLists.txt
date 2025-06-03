cmake_minimum_required(VERSION 3.16)
project(my_llama_chat)

set(CMAKE_CXX_STANDARD 17)

# --- Options ---
option(LLAMA_CUDA "Enable CUDA support" ON)

# --- Add llama.cpp sources ---
add_subdirectory(external/llama.cpp)

# --- Your executable ---
add_executable(my_llama_chat src/main.cpp)

# Include llama headers
target_include_directories(my_llama_chat PRIVATE
    external/llama.cpp
    external/llama.cpp/common
)

# --- Link llama.cpp (which includes ggml etc.) ---
target_link_libraries(my_llama_chat PRIVATE llama)

# CUDA flags
if(LLAMA_CUDA)
    target_compile_definitions(my_llama_chat PRIVATE GGML_CUDA)
endif()
