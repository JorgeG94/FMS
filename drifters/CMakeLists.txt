list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/cloud_interpolator.F90"
  "${CMAKE_CURRENT_LIST_DIR}/drifters.F90"
  "${CMAKE_CURRENT_LIST_DIR}/drifters_comm.F90"
  "${CMAKE_CURRENT_LIST_DIR}/drifters_core.F90"
  "${CMAKE_CURRENT_LIST_DIR}/drifters_input.F90"
  "${CMAKE_CURRENT_LIST_DIR}/drifters_io.F90"
  "${CMAKE_CURRENT_LIST_DIR}/quicksort.F90"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
