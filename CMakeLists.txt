cmake_minimum_required(VERSION 3.14)
project(OpenGlGenerator)

# FetchContent module is available starting from CMake 3.11
include(FetchContent)

# Set the C++ standard
set(CMAKE_CXX_STANDARD 17)

# Set the build directory
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/build)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

# Configure GLFW
FetchContent_Declare(
  GLFW
  GIT_REPOSITORY https://github.com/glfw/glfw.git
  GIT_TAG        3.3.4
)

FetchContent_GetProperties(GLFW)
if(NOT glfw_POPULATED)
  FetchContent_Populate(GLFW)
  add_subdirectory(${glfw_SOURCE_DIR} ${glfw_BINARY_DIR})
endif()


# Your executable or library target
add_executable(OpenGlGenerator src/main.cpp)

# Link GLFW and Glad to your target
target_link_libraries(OpenGlGenerator PRIVATE glfw)

# Add Glad library
set(GLAD_HEADER_FILES include/glad/glad.h include/KHR/khrplatform.h)
add_library(glad src/glad.c ${GLAD_HEADER_FILES})
target_include_directories(glad PUBLIC include)

# Link Glad to your target
target_link_libraries(OpenGlGenerator PRIVATE glad)