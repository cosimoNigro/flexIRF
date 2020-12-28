cmake_minimum_required(VERSION 3.0)
project(FLEXIRF VERSION 0.0.1)

# where to save the .so (.dylib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_INSTALL_LIBDIR ${CMAKE_BINARY_DIR}/lib) 
# where to save the executables
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
# where to find the FindPACKAGE.cmake modules
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/CMakeModules/)

find_library(CFITSIO_LIBRARY cfitsio REQUIRED)
message("-- found CFITSIO_LIBRARY at ${CFITSIO_LIBRARY}")

add_library(
	flexIRF SHARED
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRF.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFAxis.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFAxisBins.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFAxisParam.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFConfig.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFPdf.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFRange.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GIRFUtils.cc
	${PROJECT_SOURCE_DIR}/flexirf/src/GMetaData.cc
)
target_link_libraries(flexIRF ${CFITSIO_LIBRARY})
target_include_directories(flexIRF PUBLIC ${PROJECT_SOURCE_DIR}/flexirf/include)
# install the flexIRF libraries in /usr/local/lib
install(TARGETS flexIRF DESTINATION lib)

# add the executables
add_executable(simpleWriteIrf ${PROJECT_SOURCE_DIR}/exec/IRFsimpleWriteIrf.cc)
target_link_libraries(simpleWriteIrf flexIRF)
install(TARGETS simpleWriteIrf DESTINATION bin)

add_executable(simpleWriteParamIrf ${PROJECT_SOURCE_DIR}/exec/IRFsimpleWriteParamIrf.cc)
target_link_libraries(simpleWriteParamIrf flexIRF)
install(TARGETS simpleWriteParamIrf DESTINATION bin)

