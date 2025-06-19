
list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/mpp.F90"
  "${CMAKE_CURRENT_LIST_DIR}/mpp_data.F90"
  "${CMAKE_CURRENT_LIST_DIR}/mpp_domains.F90"
  "${CMAKE_CURRENT_LIST_DIR}/mpp_efp.F90"
  "${CMAKE_CURRENT_LIST_DIR}/mpp_memutils.F90"
  "${CMAKE_CURRENT_LIST_DIR}/mpp_parameter.F90"
  "${CMAKE_CURRENT_LIST_DIR}/mpp_utilities.F90"
)
list(APPEND fms_c_src_files 
  "${CMAKE_CURRENT_LIST_DIR}/mpp_memuse.c"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
