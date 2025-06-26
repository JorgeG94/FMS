#***********************************************************************
#*                   GNU Lesser General Public License
#*
#* This file is part of the GFDL Flexible Modeling System (FMS).
#*
#* FMS is free software: you can redistribute it and/or modify it under
#* the terms of the GNU Lesser General Public License as published by
#* the Free Software Foundation, either version 3 of the License, or (at
#* your option) any later version.
#*
#* FMS is distributed in the hope that it will be useful, but WITHOUT
#* ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
#* FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
#* for more details.
#*
#* You should have received a copy of the GNU Lesser General Public
#* License along with FMS.  If not, see <http://www.gnu.org/licenses/>.
#***********************************************************************

# Copyright (c) GFDL, @underwoo

cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

# Define the CMake project
project(FMS
  VERSION 2025.03.0
  DESCRIPTION  "GFDL FMS Library"
  HOMEPAGE_URL "https://www.gfdl.noaa.gov/fms"
  LANGUAGES C Fortran)

include(GNUInstallDirs)

if(NOT CMAKE_BUILD_TYPE MATCHES "^(Debug|Release|RelWithDebInfo|MinSizeRel)$")
  message(STATUS "Setting build type to 'Release' as none was specified.")
  set(CMAKE_BUILD_TYPE
      "Release"
      CACHE STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

if(NOT CMAKE_C_COMPILER_ID MATCHES "^(Intel|GNU|Clang|IntelLLVM)$")
  message(
    WARNING "Compiler not officially supported: ${CMAKE_C_COMPILER_ID}")
endif()

if(NOT CMAKE_Fortran_COMPILER_ID MATCHES "^(Intel|GNU|IntelLLVM)$")
  message(
    WARNING "Compiler not officially supported: ${CMAKE_Fortran_COMPILER_ID}")
endif()

# Append directory that contains CMake Modules for building FMS
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# Build options
option(OPENMP      "Build FMS with OpenMP support"        OFF)
option(32BIT       "Build 32-bit (r4) FMS library"         ON)
option(64BIT       "Build 64-bit (r8) FMS library"        OFF)
option(FPIC        "Build with position independent code" OFF)
option(SHARED_LIBS "Build shared/dynamic libraries"       OFF)

# Options for compiler definitions
option(INTERNAL_FILE_NML     "Enable compiler definition -DINTERNAL_FILE_NML"      ON)
option(ENABLE_QUAD_PRECISION "Enable compiler definition -DENABLE_QUAD_PRECISION"  ON)
option(PORTABLE_KINDS        "Enable compiler definition -DPORTABLE_KINDS"        OFF)
option(GFS_PHYS              "Enable compiler definition -DGFS_PHYS"              OFF)
option(LARGEFILE             "Enable compiler definition -Duse_LARGEFILE"         OFF)
option(WITH_YAML             "Enable compiler definition -Duse_yaml"              OFF)
option(USE_DEPRECATED_IO     "THIS OPTION HAS NO EFFECT AND WILL BE REMOVED IN A FUTURE RELEASE"   OFF)

if(32BIT)
  list(APPEND kinds "r4")
endif()
if(64BIT)
  list(APPEND kinds "r8")
endif()
if(NOT kinds)
  message(STATUS "Single Precision 32BIT: ${32BIT}")
  message(STATUS "Double Precision 64BIT: ${64BIT}")
  message(FATAL_ERROR "Either 32BIT or 64BIT should be ON")
endif()

# Find dependencies
find_package(MPI REQUIRED COMPONENTS C Fortran)
find_package(NetCDF REQUIRED COMPONENTS C Fortran)

# Check for the OpenMP library and set the required compile flags
if (OPENMP)
  find_package(OpenMP REQUIRED COMPONENTS C Fortran)
endif()

if (WITH_YAML)
  find_package(libyaml REQUIRED)
  include_directories(${LIBYAML_INCLUDE_DIR})
endif ()

# Enables position independent code (i.e., -fPIC)
if (FPIC)
  set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif ()
set(fms_fortran_src_files libFMS.F90)
set(fms_c_src_files "")
add_subdirectory(affinity)
add_subdirectory(amip_interp)
add_subdirectory(astronomy)
add_subdirectory(axis_utils)
add_subdirectory(block_control)
add_subdirectory(column_diagnostics)
add_subdirectory(constants)
add_subdirectory(constants4)
add_subdirectory(coupler)
add_subdirectory(data_override)
add_subdirectory(diag_integral)
add_subdirectory(diag_manager)
add_subdirectory(drifters)
add_subdirectory(exchange)
add_subdirectory(field_manager)
add_subdirectory(fms)
add_subdirectory(fms2_io)
add_subdirectory(horiz_interp)
add_subdirectory(interpolator)
add_subdirectory(memutils)
add_subdirectory(monin_obukhov)
add_subdirectory(grid_utils)
add_subdirectory(mosaic2)
add_subdirectory(mpp)
add_subdirectory(parser)
add_subdirectory(platform)
add_subdirectory(random_numbers)
add_subdirectory(sat_vapor_pres)
add_subdirectory(string_utils)
add_subdirectory(time_interp)
add_subdirectory(time_manager)
add_subdirectory(topography)
add_subdirectory(tracer_manager)
add_subdirectory(tridiagonal)

# Collect FMS header files
list(APPEND fms_header_files
  include/file_version.h
  include/fms_platform.h
)
set(fms_include_directories 
  include
  fms
  fms/include
  fms2_io/include
  string_utils/include
  mpp/include
  column_diagnostics/include
  monin_obukhov/include
  sat_vapor_pres/include
  horiz_interp/include
  diag_integral/include
  random_numbers/include
  diag_manager/include
  constants4
  topography/include
  axis_utils/include
  mosaic2/include
  constants
  astronomy/include
  field_manager/include
  time_interp/include
  tracer_manager/include
  tridiagonal/include
  interpolator/include
  coupler/include
  data_override/include
  amip_interp/include
  )

# Standard FMS compiler definitions
list(APPEND fms_defs
  use_libMPI
  use_netCDF)

# check gettid
include(CheckFunctionExists)
check_function_exists(gettid HAVE_GETTID)
if(HAVE_GETTID)
  list(APPEND fms_defs HAVE_GETTID)
endif()

# Additional (optional) compiler definitions
if(NOT CONSTANTS)
    set(CONSTANTS GFDL)
endif()
if(CONSTANTS STREQUAL "GFS")
  list(APPEND fms_defs GFS_CONSTANTS)
elseif(CONSTANTS STREQUAL "GEOS")
  list(APPEND fms_defs GEOS_CONSTANTS)
elseif(CONSTANTS STREQUAL "GFDL")
  list(APPEND fms_defs GFDL_CONSTANTS)
else()
  message(FATAL_ERROR "CONSTANTS=${CONSTANTS} option not supported")
endif()

if(GFS_PHYS)
  list(APPEND fms_defs GFS_PHYS)
endif()

if(WITH_YAML)
  list(APPEND fms_defs use_yaml)
endif()

if(USE_DEPRECATED_IO)
  message( FATAL_ERROR "FMS_IO HAS BEEN REMOVED! PLEASE UPDATE TO USE FMS2_IO AND REMOVE  "
                   "-DUSE_DEPRECATED_IO=on FROM YOUR OPTIONS OR REVERT TO VERSION 2025.02")
endif()

if(INTERNAL_FILE_NML)
  list(APPEND fms_defs INTERNAL_FILE_NML)
endif()

if(ENABLE_QUAD_PRECISION)
  list(APPEND fms_defs ENABLE_QUAD_PRECISION)
endif()

if(PORTABLE_KINDS)
  list(APPEND fms_defs PORTABLE_KINDS)
endif()

if(LARGEFILE)
  list(APPEND fms_defs use_LARGEFILE)
endif()

# Precision-based compiler definitions
if(32BIT)
  list(APPEND r4_defs OVERLOAD_R4 OVERLOAD_R8)
endif()

# Add platform specific compiler definitions
if(APPLE)
  list(APPEND fms_defs __APPLE__)
endif()

# Obtain compiler-specific flags
include(fms_compiler_flags)

# If netCDF was not built with HDF5 parallel I/O features, set up the macro -DNO_NC_PARALLEL4
IF(NOT NetCDF_PARALLEL)
  MESSAGE(WARNING "netCDF was not build with HDF5 parallel I/O features, so collective netcdf io is not allowed")
  list(APPEND fms_defs NO_NC_PARALLEL4)
ENDIF()

foreach(kind ${kinds})

  set(libTgt fms_${kind})
  set(includeDir "include_${kind}")
  set(moduleDir "${CMAKE_CURRENT_BINARY_DIR}/${includeDir}")

  # C
  add_library(${libTgt}_c OBJECT ${fms_c_src_files})

  target_include_directories(${libTgt}_c PRIVATE include
                                                 grid_utils)
  target_compile_definitions(${libTgt}_c PRIVATE "${fms_defs}")

  target_link_libraries(${libTgt}_c PRIVATE NetCDF::NetCDF_C
                                            MPI::MPI_C)

  if(OpenMP_C_FOUND)
    target_link_libraries(${libTgt}_c PRIVATE OpenMP::OpenMP_C)
  endif()

  # Fortran
  add_library(${libTgt}_f OBJECT ${fms_fortran_src_files})

  target_include_directories(${libTgt}_f PRIVATE ${fms_include_directories})

  target_compile_definitions(${libTgt}_f PRIVATE "${fms_defs}")
  target_compile_definitions(${libTgt}_f PRIVATE "${${kind}_defs}")

  set_target_properties(${libTgt}_f PROPERTIES COMPILE_FLAGS "${${kind}_flags}")

  set_target_properties(${libTgt}_f PROPERTIES Fortran_MODULE_DIRECTORY
                                               ${moduleDir})

  target_link_libraries(${libTgt}_f PRIVATE NetCDF::NetCDF_Fortran
                                            MPI::MPI_Fortran)

  if(OpenMP_Fortran_FOUND)
    target_link_libraries(${libTgt}_f PRIVATE OpenMP::OpenMP_Fortran)
  endif()

  # Check if gnu 10 or higher
  # this should only be needed with mpich, but wasn't able to find a good way to find the MPI flavor consistently
  if ( CMAKE_Fortran_COMPILER_ID MATCHES "GNU" AND CMAKE_Fortran_COMPILER_VERSION VERSION_GREATER 10.0)
    include(CheckFortranCompilerFlag)
    check_fortran_compiler_flag("-fallow-argument-mismatch" _arg_mismatch_flag)
    if(_arg_mismatch_flag)
      message(STATUS "Adding -fallow-argument-mismatch flag to compile with GCC >=10 and MPICH")
      target_compile_options(${libTgt}_f PRIVATE "-fallow-argument-mismatch;-w")
    endif()
  endif()

  # FMS (C + Fortran)
  if (SHARED_LIBS)
      message(STATUS "Shared library target: ${libTgt}")
      add_library(${libTgt} SHARED $<TARGET_OBJECTS:${libTgt}_c>
                                   $<TARGET_OBJECTS:${libTgt}_f>)
  else ()
      message(STATUS "Static library target: ${libTgt}")
      add_library(${libTgt} STATIC $<TARGET_OBJECTS:${libTgt}_c>
                                   $<TARGET_OBJECTS:${libTgt}_f>)
  endif ()

  target_include_directories(${libTgt} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/fms>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/fms/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/fms2_io/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/column_diagnostics/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/astronomy/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/monin_obukhov/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/sat_vapor_pres/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/field_manager/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/grid_utils>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/horiz_interp/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/mosaic2/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/string_utils/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/mpp/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/diag_manager/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/topography/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/random_numbers/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/diag_integral/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/diag_manager/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/tracer_manager/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/interpolator/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/coupler/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/mosaic2/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/data_override/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/tridiagonal/include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/amip_interp/include>)

  target_include_directories(${libTgt} INTERFACE
    $<BUILD_INTERFACE:${moduleDir}>
    $<INSTALL_INTERFACE:${includeDir}>)

  target_compile_definitions(${libTgt} PRIVATE "${fms_defs}")
  target_compile_definitions(${libTgt} PRIVATE "${${kind}_defs}")

  target_link_libraries(${libTgt} PUBLIC NetCDF::NetCDF_C
                                         NetCDF::NetCDF_Fortran
                                         MPI::MPI_Fortran)

  if(OpenMP_Fortran_FOUND)
    target_link_libraries(${libTgt} PRIVATE OpenMP::OpenMP_C OpenMP::OpenMP_Fortran)
  endif()

  add_library(FMS::${libTgt} ALIAS ${libTgt})

  list(APPEND LIB_TARGETS ${libTgt})
  install(DIRECTORY ${moduleDir}    DESTINATION ${CMAKE_INSTALL_PREFIX})
  install(FILES ${fms_header_files} DESTINATION ${CMAKE_INSTALL_PREFIX}/${includeDir})

endforeach()

install(
  TARGETS ${LIB_TARGETS}
  EXPORT FMSExports
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

### Package config
include(CMakePackageConfigHelpers)
set(CONFIG_INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fms)

export(EXPORT FMSExports
  NAMESPACE FMS::
  FILE fms-targets.cmake)

configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/FMSConfig.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/fms-config.cmake
  INSTALL_DESTINATION ${CONFIG_INSTALL_DESTINATION})
install(FILES ${CMAKE_SOURCE_DIR}/cmake/FindNetCDF.cmake ${CMAKE_CURRENT_BINARY_DIR}/fms-config.cmake
  DESTINATION ${CONFIG_INSTALL_DESTINATION})

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/fms-config-version.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/fms-config-version.cmake
  DESTINATION ${CONFIG_INSTALL_DESTINATION})

install(EXPORT FMSExports
  NAMESPACE FMS::
  FILE fms-targets.cmake
  DESTINATION ${CONFIG_INSTALL_DESTINATION})

# pkgconf
set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix ${CMAKE_INSTALL_PREFIX})
set(libdir ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR})
set(includedir ${CMAKE_INSTALL_PREFIX}/${includeDir})

set(CC ${CMAKE_C_COMPILER})
set(FC ${CMAKE_Fortran_COMPILER})
set(CFLAGS "${CMAKE_C_FLAGS} ${CMAKE_C_FLAGS_${CMAKE_BUILD_TYPE}}")
set(CPPFLAGS "${CMAKE_CPP_FLAGS} ${CMAKE_CPP_FLAGS_${CMAKE_BUILD_TYPE}}")
set(FCFLAGS "${CMAKE_Fortran_FLAGS} ${CMAKE_Fortran_FLAGS_${CMAKE_BUILD_TYPE}}")
set(LDFLAGS "${CMAKE_SHARED_LINKER_FLAGS} ${CMAKE_SHARED_LINKER_FLAGS_${CMAKE_BUILD_TYPE}}")

set(VERSION ${PROJECT_VERSION})

# TODO: If FMS depends on a library that is built as a static library, it
#       should be listed here as an ldflag.
set(LIBS "")

if(NOT ${NetCDF_Fortran_LIBRARY_SHARED})
  # autotools: Libs.private: -lnetcdff -lnetcdf
  string(APPEND LIBS ${NetCDF_Fortran_LIBRARIES})
endif()

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/FMS.pc.in
               ${CMAKE_CURRENT_BINARY_DIR}/FMS.pc @ONLY)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/FMS.pc
              DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
              COMPONENT utilities)
