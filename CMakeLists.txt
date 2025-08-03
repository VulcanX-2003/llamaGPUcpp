cmake_minimum_required(VERSION 3.18)
project(my_llama_chat LANGUAGES CXX CUDA)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(GGML_CUDA "Enable CUDA backend for ggml" ON)
set(BUILD_SHARED_LIBS OFF)  # 🔒 Use static lib
project(my_llama_chat LANGUAGES CXX)


set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


option(GGML_CUDA "Enable CUDA backend for ggml" ON)
set(BUILD_SHARED_LIBS OFF)  # Use static libraries for portability


if(WIN32)
    
    enable_language(CUDA) 
    
    find_package(CUDAToolkit REQUIRED)
    set(CMAKE_CUDA_STANDARD 17)
    set(CMAKE_CUDA_STANDARD_REQUIRED ON)
   
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
elseif(UNIX AND NOT APPLE)
    
    if(GGML_CUDA)
        enable_language(CUDA) 
        find_package(CUDAToolkit REQUIRED)
        set(CMAKE_CUDA_STANDARD 17)
        set(CMAKE_CUDA_STANDARD_REQUIRED ON)
    endif()
else()
    message(WARNING "Unsupported platform. Disabling CUDA.")
    set(GGML_CUDA OFF)
endif()


add_subdirectory(external/llama.cpp)

add_executable(my_llama_chat src/main.cpp)

<<<<<<< HEAD
=======

>>>>>>> f707f78a26b01121182999e9c173ac4b0158cd3b
target_include_directories(my_llama_chat PRIVATE
    external/llama.cpp
    external/llama.cpp/common
)

<<<<<<< HEAD
target_link_libraries(my_llama_chat PRIVATE llama)
=======

target_link_libraries(my_llama_chat PRIVATE llama)


if(GGML_CUDA AND CUDAToolkit_FOUND)
    target_compile_definitions(my_llama_chat PRIVATE GGML_CUDA)
    target_link_libraries(my_llama_chat PRIVATE CUDA::cudart)
endif()


if(WIN32)
    
    if(MSVC)
        set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
    endif()
elseif(UNIX AND NOT APPLE)
    
    target_compile_options(my_llama_chat PRIVATE -Wall -Wextra)
endif()
>>>>>>> f707f78a26b01121182999e9c173ac4b0158cd3b
