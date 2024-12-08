#[=====[

CMake script for building dependencies of REST

]=====]

cmake_minimum_required(VERSION 3.24)
project(rest_dep)

include(ExternalProject)

if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE RELWITHDEBINFO)
endif()

# == dftd3 ==

option(BUILD_DFTD3 "Flag to build library (libs-dftd3.so)" ON)
set(DFTD3_SRC "https://github.com/dftd3/simple-dftd3/archive/refs/tags/v1.2.1.tar.gz" CACHE STRING "URL of source code (libs-dftd3.so)")

# build with git source
if(BUILD_DFTD3)
    message(STATUS "Specified repository of source code (libs-dftd3.so): ${DFTD3_SRC}")

    ExternalProject_Add(libdftd3
        URL ${DFTD3_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        CMAKE_ARGS
            -GNinja
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
            -DCMAKE_BUILD_TYPE=RELEASE
            -DBUILD_SHARED_LIBS=1
            -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -DCMAKE_INSTALL_LIBDIR:PATH=lib
    )
endif()

# == dftd4 ==

option(BUILD_DFTD4 "Flag to build library (libdftd4.so)" ON)
set(DFTD4_SRC "https://github.com/dftd4/dftd4/archive/refs/tags/v3.7.0.tar.gz" CACHE STRING "URL of source code (libdftd4.so)")

# build with git source
if(BUILD_DFTD4)
    message(STATUS "Specified repository of source code (libdftd4.so): ${DFTD4_SRC}")

    ExternalProject_Add(libdftd4
        URL ${DFTD4_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        CMAKE_ARGS
            -GNinja
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
            -DCMAKE_BUILD_TYPE=RELEASE
            -DBUILD_SHARED_LIBS=1
            -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -DCMAKE_INSTALL_LIBDIR:PATH=lib
    )
endif()

# == hdf5 ==

option(BUILD_HDF5 "Flag to build library (libhdf5.so)" ON)
set(HDF5_SRC "https://github.com/HDFGroup/hdf5/releases/download/hdf5_1.14.5/hdf5-1.14.5.tar.gz" CACHE STRING "URL of source code (libhdf5.so)")

# build with tarball source
if(BUILD_HDF5)
    message(STATUS "Specified repository of source code (libhdf5.so): ${HDF5_SRC}")

    ExternalProject_Add(libhdf5
        URL ${HDF5_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        CMAKE_ARGS
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
            -DCMAKE_BUILD_TYPE=RELEASE
            -DBUILD_SHARED_LIBS=1
            -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -DCMAKE_INSTALL_LIBDIR:PATH=lib
    )
endif()

# == libcint ==

option(BUILD_CINT "Flag to build library (libcint.a)" ON)
set(CINT_SRC "https://github.com/sunqm/libcint/archive/refs/tags/v6.1.2.tar.gz" CACHE STRING "URL of source code (libcint.a)")

# build with git source
if(BUILD_CINT)
    message(STATUS "Specified repository of source code (libcint.a): ${CINT_SRC}")

    ExternalProject_Add(libcint
        URL ${CINT_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        CMAKE_ARGS
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
            -DCMAKE_BUILD_TYPE=RELEASE
            -DCMAKE_POSITION_INDEPENDENT_CODE=ON
            -DWITH_RANGE_COULOMB:STRING=1
            -DWITH_FORTRAN:STRING=0
            -DWITH_CINT2_INTERFACE:STRING=1
            -DENABLE_STATIC=1
            -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -DCMAKE_INSTALL_LIBDIR:PATH=lib
    )
endif()

# == librest2fch ==

option(BUILD_REST2FCH "Flag to build library (librest2fch.so)" ON)
set(REST2FCH_SRC "https://gitlab.com/jxzou/mokit/-/archive/v1.2.6/mokit-v1.2.6.tar.gz" CACHE STRING "URL of source code (librest2fch.so)")

# build with git source
if(BUILD_REST2FCH)
    message(STATUS "Specified repository of source code (librest2fch.so): ${REST2FCH_SRC}")

    ExternalProject_Add(librest2fch
        URL ${REST2FCH_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        BUILD_IN_SOURCE TRUE
        CONFIGURE_COMMAND ""
        BUILD_COMMAND
            cd ${PROJECT_BINARY_DIR}/deps/src/librest2fch/src &&
            CC=${CMAKE_C_COMPILER} FC=${CMAKE_Fortran_COMPILER} make rest2fch -f Makefile.gnu_openblas
        INSTALL_COMMAND
            mkdir -p ${PROJECT_BINARY_DIR}/deps/lib/ &&
            cp ${PROJECT_BINARY_DIR}/deps/src/librest2fch/mokit/lib/librest2fch.so ${PROJECT_BINARY_DIR}/deps/lib/
    )
endif()

# == libopenblas ==

option(BUILD_OPENBLAS "Flag to build library (libopenblas.so)" ON)
set(OPENBLAS_SRC "https://github.com/OpenMathLib/OpenBLAS/archive/refs/tags/v0.3.28.tar.gz" CACHE STRING "URL of source code (libopenblas.so)")

# build with git source
if(BUILD_OPENBLAS)
    message(STATUS "Specified repository of source code (libopenblas.so): ${OPENBLAS_SRC}")

    ExternalProject_Add(libopenblas
        URL ${OPENBLAS_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        CMAKE_ARGS
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
            -DCMAKE_BUILD_TYPE=RELEASE
            -DUSE_THREAD=1
            -DUSE_LOCKING=1
            # -DDYNAMIC_ARCH=1
            -DBUILD_SHARED_LIBS=1
            -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -DCMAKE_INSTALL_LIBDIR:PATH=lib
    )
endif()

# == libxc ==

option(BUILD_LIBXC "Flag to build library (libxc.a)" ON)
set(LIBXC_SRC "https://gitlab.com/libxc/libxc/-/archive/6.2.1/libxc-6.2.1.tar.gz" CACHE STRING "URL of source code (libxc.a)")

# build with git source
if(BUILD_LIBXC)
    message(STATUS "Specified repository of source code (libxc.a): ${LIBXC_SRC}")

    ExternalProject_Add(libxc
        URL ${LIBXC_SRC}
        PREFIX ${PROJECT_BINARY_DIR}/deps
        DOWNLOAD_EXTRACT_TIMESTAMP ON
        CMAKE_ARGS
            -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}
            -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
            -DCMAKE_BUILD_TYPE=RELEASE
            -DBUILD_SHARED_LIBS=1
            -DENABLE_FORTRAN=0
            -DDISABLE_KXC=0
            -DDISABLE_LXC=1
            -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -DCMAKE_INSTALL_LIBDIR:PATH=lib
    )
endif()

# Final install step

install(DIRECTORY ${PROJECT_BINARY_DIR}/deps/lib DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY ${PROJECT_BINARY_DIR}/deps/include DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY ${PROJECT_BINARY_DIR}/deps/bin DESTINATION ${CMAKE_INSTALL_PREFIX})
install(DIRECTORY ${PROJECT_BINARY_DIR}/deps/cmake DESTINATION ${CMAKE_INSTALL_PREFIX})

