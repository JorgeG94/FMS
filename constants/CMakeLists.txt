list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/constants.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fmsconstants.F90"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
