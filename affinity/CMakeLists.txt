list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/fms_affinity.F90"
)
list(APPEND fms_c_src_files
  "${CMAKE_CURRENT_LIST_DIR}/affinity.c"
)

# ------------------------------------------------------------------------------
# Alternative: Automatically include all *.F90 and *.c files in this directory
# Uncomment the lines below to enable globbing (CMake 3.12+ required)
#
# file(GLOB CONFIGURE_DEPENDS local_f90_sources "${CMAKE_CURRENT_LIST_DIR}/*.F90")
# file(GLOB CONFIGURE_DEPENDS local_c_sources "${CMAKE_CURRENT_LIST_DIR}/*.c")
# list(APPEND fms_fortran_src_files ${local_f90_sources})
# list(APPEND fms_c_src_files ${local_c_sources})
# ------------------------------------------------------------------------------
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
