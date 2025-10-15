# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.3] - 2025-10-15

### Added

- Build system using CMake that works on Windows, macOS and Linux.
- CI with GitHub Actions to build and run tests on Windows, macOS and Linux.
- The library can also be consumed via CMake's FetchContent module.

### Known issues

- the library may not work if used in combination with GLM (OpenGL Mathematics) due to name clashes, both in the code (headers) and the build system (CMake targets).

## [0.3.2]

<http://devernay.free.fr/hacks/glm/glm-0.3.2.tar.gz>

### Fixed

- GLUI integration issues

## [0.3.1]

<http://devernay.free.fr/hacks/glm/glm-0.3.1.tar.gz>

### Fixed

- `glm/glmimg.c`: issue a fatal error if the max texture size is <= 0
- `glm/glm.c`: disable texturing on non-textured objects
- `examples/glutobj.c` and `examples/game_glutobj.c`: fix wrong cast
- Various warnings reported by cppcheck & clang's scan-build

## [0.3.0]

<http://devernay.free.fr/hacks/glm/glm-0.3.tar.gz>

### Added

- `glmVertexNormals()`: option to add normals only where undefined
- Support for DevIL image loading (suggested by Nuno Guerreiro <nuno.guerreiro@lesium.org>)

## [0.2.0]

<http://devernay.free.fr/hacks/glm/glm-0.2.tar.gz>

### Added

- Warning/error functions in `glm_util.c`
- `glmStrStrip` function to handle filenames with spaces
- Material-by-face handling when there is more than one usemtl in a group
- Blending support from GLM_AVL (<http://www.avl.iu.edu/projects/GLM_AVL/>)
- Separated texture loading into `glmimg.c`
- Support for many more image formats (PNG & JPEG natively, plus those supported by SDL_image and simage)

### Changed

- Removed "static" from glmDraw variables to make the code reentrant
- Write OBJ files with material-by-face support

## [0.1.0]

### Added

- Initial release based on Nate Robins' glm.c
