cmake_minimum_required(VERSION 2.8.12)

if("${CMAKE_MAJOR_VERSION}" LESS 3)
   project(glmExamples C CXX)
else()
   project(glmExamples LANGUAGES C CXX)
endif()

find_package(OpenGL REQUIRED)
find_package(GLUT REQUIRED)
find_package(glui QUIET)

if(TARGET glm)
  add_library(glm::glm ALIAS glm)
else()
  find_package(glm CONFIG REQUIRED)
endif()

add_executable(game_glutobj game_glutobj.c)
target_include_directories(game_glutobj PUBLIC ${OPENGL_INCLUDE_DIR} ${GLUT_INCLUDE_DIR})
target_link_libraries(game_glutobj PUBLIC ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES} glm::glm)

add_executable(glutobj glutobj.c)
target_include_directories(glutobj PUBLIC ${OPENGL_INCLUDE_DIR} ${GLUT_INCLUDE_DIR})
target_link_libraries(glutobj PUBLIC ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES} glm::glm)

add_executable(smooth smooth.c gltb.c gltx.c smooth.c trackball.c )
target_include_directories(smooth PUBLIC ${OPENGL_INCLUDE_DIR} ${GLUT_INCLUDE_DIR})
target_link_libraries(smooth PUBLIC ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES} glm::glm)

if(glui_FOUND)
  add_executable(gluiobj gluiobj.cpp)
  target_include_directories(gluiobj PUBLIC ${OPENGL_INCLUDE_DIR} ${GLUT_INCLUDE_DIR})
  target_link_libraries(gluiobj PUBLIC ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES} glm::glm glui::glui)
  install(TARGETS gluiobj
    DESTINATION bin)
endif()


install(TARGETS game_glutobj glutobj smooth
  DESTINATION bin)
