
cmake_minimum_required(VERSION 3.20)

project(fibonacci CXX)

find_package(fmt REQUIRED)
find_package(range-v3 REQUIRED)
find_package(cxxopts REQUIRED)

set(CMAKE_CXX_STANDARD 23)

add_executable(fibo main.cxx)

target_link_libraries(fibo
    PRIVATE
    fmt::fmt
    range-v3::range-v3
    cxxopts::cxxopts)
