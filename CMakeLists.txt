cmake_minimum_required(VERSION 3.10)
project(VDB_SAMPLE)

# build option
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "-O1 -Wall")
set(CMAKE_CXX_FLAGS_DEBUG "-g3 -O0 -D_DEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG -D_RELEASE")
set(CMAKE_CXX_FLAGS_MINSIZEREL "-Os -DNDEBUG")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "-O2 -Og -DNDEBUG")
message("Build type: " ${CMAKE_BUILD_TYPE})

# build path
message("Current directory: " ${CMAKE_CURRENT_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_BUILD_TYPE})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_BUILD_TYPE})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_BUILD_TYPE})

# option
option(BUILD_AS_SHARED "whether to build as shared lib" OFF)
if(APPLE)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework IOKit")
endif()

# vcpkg
if(DEFINED ENV{VCPKG_ROOT})
    set(CMAKE_TOOLCHAIN_FILE $ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake CACHE STRING "")
    message("use vcpkg: " ${CMAKE_TOOLCHAIN_FILE})
endif()


list(APPEND SAMPLE_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)
file(GLOB SAMPLE_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

## include headers
include_directories(${SAMPLE_INCLUDE_DIR})

add_executable(sample ${SAMPLE_SOURCES})
target_link_libraries(sample)
