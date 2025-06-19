list(APPEND fms_fortran_src_files
  "${CMAKE_CURRENT_LIST_DIR}/horiz_interp_bicubic.F90"
  "${CMAKE_CURRENT_LIST_DIR}/horiz_interp_bilinear.F90"
  "${CMAKE_CURRENT_LIST_DIR}/horiz_interp_conserve.F90"
  "${CMAKE_CURRENT_LIST_DIR}/horiz_interp_spherical.F90"
  "${CMAKE_CURRENT_LIST_DIR}/horiz_interp_type.F90"
  "${CMAKE_CURRENT_LIST_DIR}/horiz_interp.F90"
)

list(APPEND fms_c_src_files
  "${CMAKE_CURRENT_LIST_DIR}/include/horiz_interp_conserve_xgrid.c"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
