cmake_minimum_required(VERSION 3.13)

project(glm VERSION 0.3.2 LANGUAGES C)

option(BUILD_EXAMPLES "Build the examples" ON)
option(BUILD_SHARED_LIBS "Build shared library" ON)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)

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
