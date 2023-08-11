cmake_minimum_required(VERSION 3.0)
project(daq_abc)

# Find LibPCAP
find_package(PkgConfig REQUIRED)
pkg_search_module(LIBDAQ REQUIRED libdaq>=3.0.11)

# List of source files
set(daq_abc_la_SOURCES
    abc_daq.h abc_daq.cc
    base_daq.h base_daq.cc
    daq_lib.h daq_lib.cc
    daq_writer.h daq_writer.cc
)

# Add the library target
add_library(daq_abc MODULE ${daq_abc_la_SOURCES})

# Set target properties
target_include_directories(daq_abc PRIVATE ${CMAKE_SOURCE_DIR}/src/common ${CMAKE_SOURCE_DIR}/src/protos)
set_target_properties(daq_abc PROPERTIES
    OUTPUT_NAME daq_abc
    PREFIX ""
    LINK_FLAGS "-export-dynamic"
)

# Link against required libraries
target_link_libraries(daq_abc
    PRIVATE
    ${CMAKE_BINARY_DIR}/src/protos/libproto.a
    ${CMAKE_BINARY_DIR}/src/common/libcommon.a
)

add_custom_target(daq_module ALL DEPENDS daq_abc)
