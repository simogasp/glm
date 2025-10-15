cmake_minimum_required(VERSION 3.13)

project(glm VERSION 0.3.2 LANGUAGES C)

option(BUILD_EXAMPLES "Build the examples" ON)
option(BUILD_SHARED_LIBS "Build shared library" ON)
set(GLM_C_STANDARD 17)
set(GLM_EXISTING_C_STANDARDS 90 99 11 17 23)
if(NOT GLM_C_STANDARD IN_LIST GLM_EXISTING_C_STANDARDS)
  message(FATAL_ERROR "Unsupported C++ standard: ${GLM_C_STANDARD}. Supported standards are: ${GLM_EXISTING_C_STANDARDS}")
endif()

if(NOT DEFINED CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD ${GLM_C_STANDARD})
  set(CMAKE_C_STANDARD_REQUIRED ON)
  set(CMAKE_C_EXTENSIONS OFF)
endif()
message(STATUS "Using C standard: ${CMAKE_C_STANDARD}")

set(GLM_CXX_FEATURE c_std_${CMAKE_C_STANDARD})

if(BUILD_SHARED_LIBS)
    if(WIN32)
        # export all symbols on a windows build of dynamic libs
        set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS  ON)
    endif()
endif()

add_subdirectory(glm)
if(BUILD_EXAMPLES)
  add_subdirectory(examples)
endif()
