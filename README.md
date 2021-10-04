# MUMPS example via CMake FetchContent

Example of using MUMPS via CMake
[FetchContent](https://cmake.org/cmake/help/latest/module/FetchContent.html).

This example will download and build MUMPS, and find or build Scalapack, BLACS, and LAPACK.

```sh
cmake -B build

cmake --build build

ctest --test-dir build
```
