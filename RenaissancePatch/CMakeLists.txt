cmake_minimum_required(VERSION 3.10)
project(RenaissancePatch)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

# === GCC/CMake Build Instructions ===
#
# 1. Make sure GCC (MinGW-w64) is installed and added to your PATH.
# 2. Run CMake with the MinGW Makefiles generator:
#      cmake -G "MinGW Makefiles" -DCMAKE_BUILD_TYPE=Release ..
# 3. Build with:
#      mingw32-make
#
# If using MSVC, open a Developer Command Prompt and use the default generator.
#
# === x86 and Windows 2000/XP Compatibility ===
# This project builds for x86 only. Windows 2000/XP compatibility is enabled.

# Force x86 build
set(CMAKE_GENERATOR_PLATFORM x86)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")

# Set minimum Windows version for compatibility
add_definitions(-D_WIN32_WINNT=0x0500) # 0x0500 = Windows 2000, 0x0501 = Windows XP

# Source files
set(SOURCES
    dllmain.cpp
)

# Include directories
include_directories(
    include
)

# Library directories
link_directories(
    lib
)

# Add DLL target
add_library(RenaissancePatch SHARED ${SOURCES})

# Link MinHook and ws2_32
if(MINGW)
    target_link_libraries(RenaissancePatch MinHook.x86 ws2_32)
else()
    target_link_libraries(RenaissancePatch MinHook.x86.lib ws2_32)
endif()

# Output directories
set_target_properties(RenaissancePatch PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib
)
