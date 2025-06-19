list(APPEND fms_fortran_src_files
"${CMAKE_CURRENT_LIST_DIR}/atmos_ocean_fluxes.F90"
"${CMAKE_CURRENT_LIST_DIR}/gex.F90"
"${CMAKE_CURRENT_LIST_DIR}/coupler_types.F90"
"${CMAKE_CURRENT_LIST_DIR}/ensemble_manager.F90"
)
set(fms_fortran_src_files "${fms_fortran_src_files}" PARENT_SCOPE)
set(fms_c_src_files "${fms_c_src_files}" PARENT_SCOPE)
