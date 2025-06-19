list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/blackboxio.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_io_utils.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_netcdf_domain_io.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_netcdf_unstructured_domain_io.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms2_io.F90"
  "${CMAKE_CURRENT_LIST_DIR}/netcdf_io.F90"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
