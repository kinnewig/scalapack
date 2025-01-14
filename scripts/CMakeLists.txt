cmake_minimum_required(VERSION 3.19)

project(BuildLAPACK
LANGUAGES Fortran
)

include(ExternalProject)

option(BUILD_SINGLE "Build single precision real" ON)
option(BUILD_DOUBLE "Build double precision real" ON)
option(BUILD_COMPLEX "Build single precision complex")
option(BUILD_COMPLEX16 "Build double precision complex")

set(lapack_cmake_args
-DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_INSTALL_PREFIX}
-DBUILD_SHARED_LIBS:BOOL=${BUILD_SHARED_LIBS}
-DCMAKE_BUILD_TYPE=Release
-DBUILD_TESTING:BOOL=false
-DBUILD_SINGLE:BOOL=${BUILD_SINGLE}
-DBUILD_DOUBLE:BOOL=${BUILD_DOUBLE}
-DBUILD_COMPLEX:BOOL=${BUILD_COMPLEX}
-DBUILD_COMPLEX16:BOOL=${BUILD_COMPLEX16}
)

file(READ ${CMAKE_CURRENT_SOURCE_DIR}/../cmake/libraries.json json)
string(JSON lapack_url GET ${json} lapack git)
string(JSON lapack_tag GET ${json} lapack tag)

ExternalProject_Add(LAPACK
GIT_REPOSITORY ${lapack_url}
GIT_TAG ${lapack_tag}
CMAKE_ARGS ${lapack_cmake_args}
INACTIVITY_TIMEOUT 60
TLS_VERIFY true
)
