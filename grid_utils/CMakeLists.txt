list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/gradient.F90"
)
list(APPEND fms_c_src_files 
  "${CMAKE_CURRENT_LIST_DIR}/gradient_c2l.c"
  "${CMAKE_CURRENT_LIST_DIR}/grid_utils.c"
  "${CMAKE_CURRENT_LIST_DIR}/tree_utils.c")
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
