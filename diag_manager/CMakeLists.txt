
list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/diag_axis.F90"
  "${CMAKE_CURRENT_LIST_DIR}/diag_data.F90"
  "${CMAKE_CURRENT_LIST_DIR}/diag_grid.F90"
  "${CMAKE_CURRENT_LIST_DIR}/diag_manager.F90"
  "${CMAKE_CURRENT_LIST_DIR}/diag_output.F90"
  "${CMAKE_CURRENT_LIST_DIR}/diag_table.F90"
  "${CMAKE_CURRENT_LIST_DIR}/diag_util.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_time_utils.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_object.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_yaml.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_file_object.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_field_object.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_axis_object.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_output_buffer.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_input_buffer.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_time_reduction.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_outfield.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_elem_weight_procs.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_fieldbuff_update.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_bbox.F90"
  "${CMAKE_CURRENT_LIST_DIR}/fms_diag_reduction_methods.F90"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
