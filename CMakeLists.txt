cmake_minimum_required(VERSION 2.8.12)

option(BUILD_EXAMPLES "Build the examples" ON)

add_subdirectory(glm)
if(BUILD_EXAMPLES)
  add_subdirectory(examples)
endif()
