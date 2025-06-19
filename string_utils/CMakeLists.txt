list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/fms_string_utils.F90"
)
list(APPEND fms_c_src_files 
  "${CMAKE_CURRENT_LIST_DIR}/fms_string_utils_binding.c")
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
