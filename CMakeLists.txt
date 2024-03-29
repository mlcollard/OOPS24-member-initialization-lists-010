# @file CMakeLists.txt
# 
# CMake files for member initialization list examples

cmake_minimum_required(VERSION 3.25)

# project info
project(InitList)

# Force debug build since used for exploring behavior
set(CMAKE_BUILD_TYPE Debug)

# C++17
set(CMAKE_CXX_STANDARD 17)

# Turn on warnings
if (MSVC)
    # warning level 4
    add_compile_options(/W4)
else()
    # standard warnings
    add_compile_options(-Wall -Wno-unused-private-field)
endif()

# Assume each .cpp file is a separate application
file(GLOB APPLICATIONS "*.cpp")

# Generate executable for each .cpp file
foreach(APP ${APPLICATIONS})

    # Use basename (no path, no extension) of .cpp file for application name
    get_filename_component(APPBASENAME "${APP}" NAME_WLE)

    # Application is based on .cpp file only
    add_executable(${APPBASENAME})
    target_sources(${APPBASENAME} PRIVATE ${APPBASENAME}.cpp)
    target_compile_features(${APPBASENAME} PRIVATE cxx_std_17)
    target_compile_options(${APPBASENAME} PRIVATE
        $<$<CXX_COMPILER_ID:MSVC>:/Wall>
        $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-Wall>
    )

endforeach()
