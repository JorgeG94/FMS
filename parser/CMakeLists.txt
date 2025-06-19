list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/yaml_parser.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_yaml_output.F90")

list(APPEND fms_c_src_files
  "${CMAKE_CURRENT_LIST_DIR}/yaml_parser_binding.c"
  "${CMAKE_CURRENT_LIST_DIR}/yaml_output_functions.c")
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
