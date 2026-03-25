cmake_minimum_required(VERSION 3.31)

# MSVC 运行时库策略（必须在 project() 之前设置）
cmake_policy(SET CMP0091 NEW)

project(HelloWorld)

# ============ 全局设置 ============
# C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# MSVC 静态运行时
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# ============ 子目录 ============
add_subdirectory(helloworld)