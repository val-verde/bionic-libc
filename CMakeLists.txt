cmake_minimum_required(VERSION 3.10)
set(BIONIC_LIBC_VERSION 1)
project(bionic_libc VERSION ${BIONIC_LIBC_VERSION} LANGUAGES ASM C CXX)

if (${CMAKE_SYSTEM_PROCESSOR} STREQUAL "aarch64")
    set(bionic_libc_ARCH arm64)
elseif (${CMAKE_SYSTEM_PROCESSOR} STREQUAL "x86_64")
    set(bionic_libc_ARCH x86_64)
endif ()


set(CMAKE_ASM_FLAGS "-D__ASSEMBLY__=1")
set(CMAKE_C_FLAGS "-DJEMALLOC_NO_DEMANGLE=1 -DJEMALLOC_NO_RENAME=1")
set(CMAKE_CXX_FLAGS "-DJEMALLOC_NO_DEMANGLE=1 -DJEMALLOC_NO_RENAME=1")

file(
    GLOB bionic_libc_SOURCES
    libc/arch-${bionic_libc_ARCH}/*.c
    libc/arch-${bionic_libc_ARCH}/*.cpp
    libc/arch-${bionic_libc_ARCH}/*.S
    libc/arch-${bionic_libc_ARCH}/bionic/*.c
    libc/arch-${bionic_libc_ARCH}/bionic/*.cpp
    libc/arch-${bionic_libc_ARCH}/bionic/*.S
    libc/bionic/*.c
    libc/bionic/*.cpp
)

set(
    bionic_libc_INCLUDES
    libc
    libc/async_safe/include
    libc/include
    libc/platform
    libc/system_properties/include
    libc/stdio
)

add_library(
    libc SHARED
    ${bionic_libc_SOURCES}
)

target_include_directories(
    libc
    PUBLIC
    ${bionic_libc_INCLUDES}
)

install(
    TARGETS libc LIBRARY DESTINATION lib
)

