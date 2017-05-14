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
  message(STATUS "Found DevIL")
  set(HAVE_DEVIL ON)
endif()

set(HAVE_LIBSDL_IMAGE OFF)
find_package(SDL_image QUIET)
if(SDLIMAGE_FOUND)
  message(STATUS "Found SDL_image")
  set(HAVE_LIBSDL_IMAGE ON)
endif()

## Automatically generated files are stored here
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(SOURCE_FILES
  glm.c
  glm_util.c
  glmimg.c
  glmimg_devil.c
  glmimg_jpg.c
  glmimg_png.c
  glmimg_sdl.c
  glmimg_sim.c)

add_library(glm ${SOURCE_FILES})

target_include_directories(glm
  PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/glm>"
  "$<BUILD_INTERFACE:${generated_dir}>"
  "$<INSTALL_INTERFACE:include/>"
  "$<INSTALL_INTERFACE:include/glm>"
  ${OPENGL_INCLUDE_DIR})
target_link_libraries(glm
  PUBLIC ${OPENGL_LIBRARIES}
  PRIVATE m)

target_compile_definitions(glm PRIVATE "HAVE_CONFIG_H")

if(HAVE_DEVIL)
  target_include_directories(glm PRIVATE ${IL_INCLUDE_DIR})
  target_link_libraries(glm PRIVATE ${IL_LIBRARIES})
endif()
if(HAVE_LIBPNG)
  target_include_directories(glm PRIVATE ${PNG_INCLUDE_DIRS})
  target_link_libraries(glm PRIVATE ${PNG_LIBRARIES})
endif()
if(HAVE_LIBJPEG)
  target_include_directories(glm PRIVATE ${JPEG_INCLUDE_DIR})
  target_link_libraries(glm PRIVATE ${JPEG_LIBRARIES})
endif()

set(configfile "${generated_dir}/config.h")
configure_file("${CMAKE_SOURCE_DIR}/cmake/config.h.in"
               "${configfile}" @ONLY)

set_target_properties(glm PROPERTIES
  DEBUG_POSTFIX "d"
  VERSION ${PROJECT_VERSION}
  SOVERSION ${PROJECT_VERSION})

####
# Installation
set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include/glm")

# Configuration
set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

# Include module with fuction 'write_basic_package_version_file'
include(CMakePackageConfigHelpers)

# Configure '<PROJECT-NAME>ConfigVersion.cmake'
write_basic_package_version_file(
    "${version_config}" COMPATIBILITY SameMajorVersion
)

# Configure '<PROJECT-NAME>Config.cmake'
configure_package_config_file(
    "${CMAKE_SOURCE_DIR}/cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION "${config_install_dir}"
)

# Targets:
install(
    TARGETS glm
    EXPORT "${targets_export_name}"
    LIBRARY DESTINATION "lib"
    ARCHIVE DESTINATION "lib"
    RUNTIME DESTINATION "bin"
    INCLUDES DESTINATION "${include_install_dir}"
)

# Headers:
install(
    FILES "glm.h" ${configfile}
    DESTINATION "${include_install_dir}"
)


# Config
install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)

# Config
install(
    EXPORT "${targets_export_name}"
    NAMESPACE "${namespace}"
    DESTINATION "${config_install_dir}"
)
