# Top level CMakeLists.txt file.  Normally, you will not have to mess
# with this but if you are interested in the test suite and/or coverage
# then you can party hard.  The setup here assumes that this
# will be used in an out of source build.  That is, use the following
# (assuming current directory is <root>/deps/src)
#
#    mkdir build
#    cd build
#    cmake ..
#    make         # this makes only the shared library for nl2sol
#                   and installs it at .
#
# to run the test suite
#    make test
# 
# this will only generally show:
#------------------------------------------------------
#Running tests...
#Test project /home/ubuntu/Nl2sol.jl/deps/src/build
#    Start 1: nlmain
#1/1 Test #1: nlmain ...........................   Passed    0.05 sec
#
#100% tests passed, 0 tests failed out of 1
#
#Total Test time (real) =   0.05 sec
#------------------------------------------------------
#
# The detailed results can be found in 
#
#        build/Testing/Temporary/LastTest.log
#
# For a coverage measurement using the same test suite, first remove all contents
# from build directory. Then issue:
#
#    cmake .. -DCMAKE_BUILD_TYPE=COVERAGE
#    make
#    make test
#
#  The coverage files for the nl2sol shared library (the *.gcda and *.gcno) 
#  can now be found in build/nl2sol/CMakeFiles/nl2sol.dir
#
#  and you can see the coverage by running gcov in that directory
#
#     gcov *.o
# > ...
# > Lines executed:87.87% of 1871
#
# Coverage for the test driver itself is in
#
#  build/tests/CMakeFiles/nlmain.dir
#
# NOTE: don't forget to do a clean rebuild of the library after measuring
# the coverage.  Using coverage enabled libraries in production is not a
# good idea.

cmake_minimum_required (VERSION 2.8)
project (nl2sol)
enable_language (Fortran)
enable_testing()


# make sure that the default is a RELEASE
if (NOT CMAKE_BUILD_TYPE)
  set (CMAKE_BUILD_TYPE RELEASE CACHE STRING
      "Choose the type of build, options are: None Debug Release."
      FORCE)
endif (NOT CMAKE_BUILD_TYPE)

message ("Build type: " ${CMAKE_BUILD_TYPE})

# default installation
get_filename_component (default_prefix "." ABSOLUTE)
set (CMAKE_INSTALL_PREFIX ${default_prefix} CACHE STRING
      "Choose the installation directory; by default it installs in the Nl2sol.f directory."
      FORCE)
message ("CMAKE_INSTALL_PREFIX: " ${CMAKE_INSTALL_PREFIX})

# The version number. This is the version number of the netlib downloaded NL2SOL
set (Nl2sol.jl_VERSION_MAJOR 2)
set (Nl2sol.jl_VERSION_MINOR 2)

# FFLAGS depend on the compiler
get_filename_component (Fortran_COMPILER_NAME ${CMAKE_Fortran_COMPILER} NAME)
message ("Fortran compiler: " ${Fortran_COMPILER_NAME})

set (BUILD_SHARED_LIBS 1)

if (Fortran_COMPILER_NAME MATCHES "gfortran.*")
    # gfortran
    set (CMAKE_Fortran_FLAGS_RELEASE "-funroll-all-loops -O3")
    set (CMAKE_Fortran_FLAGS_DEBUG   "-O0 -g -fbounds-check")
    set (CMAKE_Fortran_FLAGS_COVERAGE   "-O2 --coverage -fPIC")
# Don't yet know the appropriate flags for the Intel compiler or g77
# elseif (Fortran_COMPILER_NAME MATCHES "ifort.*")
#     # ifort (untested)
#     set (CMAKE_Fortran_FLAGS_RELEASE "-f77rtl -O3")
#     set (CMAKE_Fortran_FLAGS_DEBUG   "-f77rtl -O0 -g")
# elseif (Fortran_COMPILER_NAME MATCHES "g77")
#     # g77
#     set (CMAKE_Fortran_FLAGS_RELEASE "-funroll-all-loops -fno-f2c -O3 -m32")
#     set (CMAKE_Fortran_FLAGS_DEBUG   "-fno-f2c -O0 -g -m32")
endif (Fortran_COMPILER_NAME MATCHES "gfortran.*")

set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_INSTALL_PREFIX})
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_INSTALL_PREFIX})

if (CMAKE_BUILD_TYPE MATCHES "COVERAGE")
    message("WARNING: This is a coverage build. Remember to rebuild a clean lib when done")
endif ()

add_subdirectory(src)
add_subdirectory(tests)


