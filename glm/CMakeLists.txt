cmake_minimum_required(VERSION 3.1.3)

project(glm VERSION 0.3.2 LANGUAGES C)

find_package(OpenGL REQUIRED)

set(HAVE_LIBPNG OFF)
find_package(PNG QUIET)
if(PNG_FOUND)
  message(STATUS "Found libpng: ${PNG_VERSION_STRING}")
  set(HAVE_LIBPNG ON)
endif()

set(HAVE_LIBJPEG OFF)
find_package(JPEG QUIET)
if(JPEG_FOUND)
  message(STATUS "Found libjpeg")
  set(HAVE_LIBJPEG ON)
endif()

set(HAVE_DEVIL OFF)
find_package(DevIL QUIET)
if(IL_FOUND OR DevIL_FOUND)
  message(STATUS "Found DevIL ${IL_INCLUDE_DIR}")
  set(HAVE_DEVIL ON)
endif()

## Automatically generated files
set(generatedDir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(configfile "${generatedDir}/config.h")
configure_file("${CMAKE_SOURCE_DIR}/cmake/config.h.in"
               "${configfile}" @ONLY)
