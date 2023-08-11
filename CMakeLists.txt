cmake_minimum_required(VERSION 3.9)
project(abcip CXX)

set(CMAKE_CXX_STANDARD 14)

add_subdirectory(src/app)
add_subdirectory(src/common)
add_subdirectory(src/protos)
add_subdirectory(src/daq)
