# GLM

##  Dependencies

GLM depends only on OpenGL. Optionally, the library offer some functions to load different types of images to load the texture files. Some optional dependencies that are supported are:

* libpng
* libjpeg
* DevIL
* SDL_image
* Simage

The examples that are provided show how the library can be used to load obj files and visualize them with different UI systems. The examples depends on

* OpenGL
* libglut / freeglut
* libglui

If the relevant libraries are not found the examples are not built.

## Building Instructions

There are two possible building methods, one using the classic autoconf and automake tools, or using CMake.

### autoconf

Just execute the script `autogen.sh` and then
```bash
make -j4
make install
```

Note that `autogen.sh` forward all the parameters to `config`. So, if you want
to install the library in a different location than the default one (`/usr/local`),
you can pass `--prefix=path/to/install`.

## CMake

CMake has been tested on Linux and OsX, the minimum required version is 2.8.12.
In order to build the library you can do

```bash
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX:PATH=path/to/install
make install -j4
```

In order to use the library as a third party in a cmake project you can add this to your `CMakeLists.txt`:

```cmake
# Find the package from the glmConfig.cmake
# in <prefix>/lib/cmake/glm/. Under the namespace glm::
# it exposes the target glm that allows you to compile
# and link with the library
find_package(glm CONFIG REQUIRED)
...
# suppose you want to try it out in a executable
add_executable(glmtest yourfile.cpp)
# add link to the library
target_link_libraries(glmtest PUBLIC glm::glm)
```
Check the `CMakeLists.txt` in the examples folder for an example.

When you install glm, a file `glmConfig.cmake` is installed in `path/to/install/lib/cmake/glm/` that allows you to import the library in your CMake project by passing the location of `glmConfig.cmake` from the cmake command line:

```bash
cmake .. -Dglm_DIR=path/to/install/lib/cmake/glm/
```
