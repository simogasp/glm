cmake_minimum_required(VERSION 3.13)

if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(GLM_IS_MAIN_PROJECT ON)
else()
    set(GLM_IS_MAIN_PROJECT OFF)
endif()

project(glm VERSION 0.3.3 LANGUAGES C)
set(glm_VERSION ${PROJECT_VERSION})
message(STATUS "GLM version: ${glm_VERSION}")

if(GLM_IS_MAIN_PROJECT)
    set(GLM_INSTALL_DEFAULT ON)
    set(GLM_BUILD_EXAMPLES_DEFAULT ON)
else()
    set(GLM_INSTALL_DEFAULT OFF)
    set(GLM_BUILD_EXAMPLES_DEFAULT OFF)
endif()

option(GLM_BUILD_EXAMPLES "Build the examples" ${GLM_BUILD_EXAMPLES_DEFAULT})
option(BUILD_SHARED_LIBS "Build shared library" ON)
option(GLM_INSTALL "Install GLM library" ${GLM_INSTALL_DEFAULT})

set(GLM_C_STANDARD 17)
set(GLM_EXISTING_C_STANDARDS 90 99 11 17 23)
if(NOT GLM_C_STANDARD IN_LIST GLM_EXISTING_C_STANDARDS)
  message(FATAL_ERROR "Unsupported C++ standard: ${GLM_C_STANDARD}. Supported standards are: ${GLM_EXISTING_C_STANDARDS}")
endif()

if(NOT DEFINED CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD ${GLM_C_STANDARD})
  set(CMAKE_C_STANDARD_REQUIRED ON)
  # this MUST be ON otherwise there are segfaults
  set(CMAKE_C_EXTENSIONS ON)
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
if(GLM_BUILD_EXAMPLES)
  add_subdirectory(examples)
endif()
