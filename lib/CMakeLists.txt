# Copyright 2011,2012,2016,2018,2019 Free Software Foundation, Inc.
#
# This file was generated by gr_modtool, a tool from the GNU Radio framework
# This file is a part of gr-cuda_buffer
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Setup library
########################################################################
include(GrPlatform) #define LIB_SUFFIX

###############################################################################
# CUDA Dependency is required
###############################################################################
find_package(CUDA REQUIRED)


list(APPEND cuda_buffer_sources
    cuda_buffer.cc
)

set(cuda_buffer_sources "${cuda_buffer_sources}" PARENT_SCOPE)
if(NOT cuda_buffer_sources)
    MESSAGE(STATUS "No C++ sources... skipping lib/")
    return()
endif(NOT cuda_buffer_sources)

add_library(gnuradio-cuda_buffer SHARED ${cuda_buffer_sources})
target_link_libraries(gnuradio-cuda_buffer PUBLIC
    gnuradio::gnuradio-runtime
    ${CUDA_CUDART_LIBRARY}  
    )

target_include_directories(gnuradio-cuda_buffer
    PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
    PUBLIC $<INSTALL_INTERFACE:include>
    ${CUDA_INCLUDE_DIRS} 
  )
set_target_properties(gnuradio-cuda_buffer PROPERTIES DEFINE_SYMBOL "gnuradio_cuda_buffer_EXPORTS")

if(APPLE)
    set_target_properties(gnuradio-cuda_buffer PROPERTIES
        INSTALL_NAME_DIR "${CMAKE_INSTALL_PREFIX}/lib"
    )
endif(APPLE)

########################################################################
# Install built library files
########################################################################
include(GrMiscUtils)
GR_LIBRARY_FOO(gnuradio-cuda_buffer)

########################################################################
# Print summary
########################################################################
message(STATUS "Using install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "Building for version: ${VERSION} / ${LIBVER}")

########################################################################
# Build and register unit test
########################################################################
include(GrTest)

# If your unit tests require special include paths, add them here
#include_directories()
# List all files that contain Boost.UTF unit tests here
list(APPEND test_cuda_buffer_sources
)
# Anything we need to link to for the unit tests go here
list(APPEND GR_TEST_TARGET_DEPS gnuradio-cuda_buffer)

if(NOT test_cuda_buffer_sources)
    MESSAGE(STATUS "No C++ unit tests... skipping")
    return()
endif(NOT test_cuda_buffer_sources)

foreach(qa_file ${test_cuda_buffer_sources})
    GR_ADD_CPP_TEST("cuda_buffer_${qa_file}"
        ${CMAKE_CURRENT_SOURCE_DIR}/${qa_file}
    )
endforeach(qa_file)
