cmake_minimum_required(VERSION 3.20...3.21)

project(MUMPS_example
LANGUAGES C Fortran
)

enable_testing()

include(FetchContent)

set(CMAKE_TLS_VERIFY true)
set(FETCHCONTENT_UPDATES_DISCONNECTED_MUMPSUPSTREAM true)

# --- auto-ignore build directory
if(NOT EXISTS ${PROJECT_BINARY_DIR}/.gitignore)
  file(WRITE ${PROJECT_BINARY_DIR}/.gitignore "*")
endif()

# --- build MUMPS

find_package(MPI COMPONENTS C Fortran REQUIRED)

FetchContent_Declare(MUMPSupstream
  GIT_REPOSITORY https://github.com/scivision/mumps.git
  GIT_TAG v5.4.1.2
)

FetchContent_MakeAvailable(MUMPSupstream)

# Example C and Fortran main programs using MUMPS

add_executable(c_mumps src/d_simple.c)
target_link_libraries(c_mumps PRIVATE MUMPS::MUMPS MPI::MPI_C)
add_test(NAME MUMPS_C
COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} 2 $<TARGET_FILE:c_mumps>
)

add_executable(fortran_mumps src/d_simple.f90)
target_link_libraries(fortran_mumps PRIVATE MUMPS::MUMPS MPI::MPI_Fortran)
add_test(NAME MUMPS_Fortran
COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} 2 $<TARGET_FILE:fortran_mumps>
)
