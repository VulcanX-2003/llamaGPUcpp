cmake_minimum_required(VERSION 3.18)
project(chatTerm LANGUAGES CXX CUDA)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(GGML_CUDA "Enable CUDA backend for ggml" ON)
set(BUILD_SHARED_LIBS OFF)  # 🔒 Use static lib
project(chatTerm LANGUAGES CXX)


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

add_executable(chatTerm src/main.cpp)


target_include_directories(chatTerm PRIVATE
    external/llama.cpp
    external/llama.cpp/common
)

target_link_libraries(chatTerm PRIVATE llama)


target_link_libraries(chatTerm PRIVATE llama)


if(GGML_CUDA AND CUDAToolkit_FOUND)
    target_compile_definitions(chatTerm PRIVATE GGML_CUDA)
    target_link_libraries(chatTerm PRIVATE CUDA::cudart)
endif()


if(WIN32)
    
    if(MSVC)
        set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
    endif()
elseif(UNIX AND NOT APPLE)
    
    target_compile_options(chatTerm PRIVATE -Wall -Wextra)
endif()

