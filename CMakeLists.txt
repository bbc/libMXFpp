cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

include("${CMAKE_CURRENT_LIST_DIR}/cmake/options.cmake")

if(MSVC AND LIBMXFPP_SET_MSVC_RUNTIME AND CMAKE_VERSION VERSION_GREATER_EQUAL 3.15.0)
    # cmake version >= 3.15: Use MultiThreadedDLL runtime
    cmake_policy(SET CMP0091 NEW)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>DLL")
endif()

project(libMXF++
    VERSION 1.0.3
    DESCRIPTION "C++ wrapper library for libMXF that supports reading and writing the SMPTE ST 377-1 MXF file format"
    HOMEPAGE_URL https://github.com/bbc/libMXFpp
    LANGUAGES CXX
)
set(PROJECT_VERSION_RELEASE 2)

if(MSVC)
    add_compile_options(/W3)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)

    if(LIBMXFPP_SET_MSVC_RUNTIME AND CMAKE_VERSION VERSION_LESS 3.15.0)
        # cmake version < 3.15: Update compiler flags to use the MultiThreadedDLL runtime
        macro(update_msvc_runtime_flags flags)
            string(REGEX REPLACE "/MT" "/MD" ${flags} "${${flags}}")
        endmacro()

        update_msvc_runtime_flags(CMAKE_C_FLAGS)
        update_msvc_runtime_flags(CMAKE_C_FLAGS_DEBUG)
        update_msvc_runtime_flags(CMAKE_C_FLAGS_RELEASE)
        update_msvc_runtime_flags(CMAKE_CXX_FLAGS)
        update_msvc_runtime_flags(CMAKE_CXX_FLAGS_DEBUG)
        update_msvc_runtime_flags(CMAKE_CXX_FLAGS_RELEASE)
    endif()
else()
    add_compile_options(-W -Wall -O2)
endif()

if(LIBMXFPP_BUILD_TESTING AND (NOT DEFINED BUILD_TESTING OR BUILD_TESTING))
    enable_testing()
endif()

include("${PROJECT_SOURCE_DIR}/cmake/libmxf.cmake")

configure_file(config.h.in config.h)

add_subdirectory(libMXF++)
add_subdirectory(test)
add_subdirectory(examples)
add_subdirectory(tools)
