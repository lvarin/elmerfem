INCLUDE(test_macros)
INCLUDE_DIRECTORIES(${CMAKE_BINARY_DIR}/fem/src)

CONFIGURE_FILE( fsi.sif fsi.sif COPYONLY)
ADD_ELMERTEST_MODULE(fsi_beam_nodalforce FsiStuff FsiStuff.f90)

file(COPY ELMERSOLVER_STARTINFO fsi.grd  FsiStuff.f90 DESTINATION "${CMAKE_CURRENT_BINARY_DIR}/")

ADD_ELMER_TEST(fsi_beam_nodalforce LABELS elasticsolve fsi)
