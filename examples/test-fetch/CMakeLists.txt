# a simple test to see if a project can consume the library via fetchcontent
cmake_minimum_required(VERSION 3.13)
project(test-fetch LANGUAGES CXX C)

find_package(glm QUIET)
if(NOT glm_FOUND)
    message(STATUS "glm not found, fetching it")
    include(FetchContent)
    FetchContent_Declare(
      glm
      GIT_REPOSITORY https://github.com/simogasp/glm.git
            GIT_TAG v0.3.3)
    FetchContent_MakeAvailable(glm)
endif()
message(STATUS "GLM version: ${glm_VERSION}")
add_executable(test-fetch test-fetch.cpp)
target_link_libraries(test-fetch PRIVATE glm::glm)