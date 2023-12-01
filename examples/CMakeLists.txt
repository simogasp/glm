cmake_minimum_required(VERSION 3.13)

project(glmExamples LANGUAGES C CXX)

find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)
find_package(glui QUIET)

if(TARGET glm)
  add_library(glm::glm ALIAS glm)
else()
  find_package(glm CONFIG REQUIRED)
endif()
set(EXECUTABLE_TARGETS "")

add_executable(game_glutobj game_glutobj.c)
target_link_libraries(game_glutobj PUBLIC OpenGL::GL OpenGL::GLU GLUT::GLUT glm::glm)
list(APPEND EXECUTABLE_TARGETS "game_glutobj")

add_executable(glutobj glutobj.c)
target_link_libraries(glutobj PUBLIC OpenGL::GL OpenGL::GLU GLUT::GLUT glm::glm)
list(APPEND EXECUTABLE_TARGETS "glutobj")

add_executable(smooth smooth.c gltb.c gltx.c smooth.c trackball.c )
target_link_libraries(smooth PUBLIC OpenGL::GL OpenGL::GLU GLUT::GLUT glm::glm)
list(APPEND EXECUTABLE_TARGETS "smooth")

if(glui_FOUND)
  add_executable(gluiobj gluiobj.cpp)
  target_link_libraries(gluiobj PUBLIC OpenGL::GL OpenGL::GLU GLUT::GLUT glm::glm glui::glui)
  list(APPEND EXECUTABLE_TARGETS "gluiobj")
  install(TARGETS gluiobj
    DESTINATION bin)
endif()

if(APPLE)
    foreach(target ${EXECUTABLE_TARGETS})
        target_compile_definitions(${target} PUBLIC GL_SILENCE_DEPRECATION)
    endforeach()
endif()

if(NOT MSVC)
    foreach(target ${EXECUTABLE_TARGETS})
        target_link_libraries(${target} PRIVATE m)
    endforeach()
endif()


install(TARGETS game_glutobj glutobj smooth
  DESTINATION bin)
