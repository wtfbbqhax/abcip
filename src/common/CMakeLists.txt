cmake_minimum_required(VERSION 3.9)
project(common)
set(CMAKE_CXX_STANDARD 14)


# Add the library target
add_library(
    common
    STATIC
)

target_compile_options(
    common
    PRIVATE
    -fPIC
)

# List of source files
target_sources(
    common
    PRIVATE
    abc_io.cc abc_io.h
    abc_ip.cc abc_ip.h
    attribute.cc attribute.h
    cake.cc cake.h
    cmd_parser.cc cmd_parser.h
    command.h
    data.cc data.h
    data_parser.cc data_parser.h
    dumb_user.cc dumb_user.h
    field.cc field.h
    packet.h
    parser.h
    pile.cc pile.h
    pseudo_hdr.h pseudo_hdr.cc
    reader.h
    status.cc status.h
    stream_reader.cc stream_reader.h
    user.cc user.h
    writer.h
)

# Set include directories
target_include_directories(
    common
    PRIVATE
    ${CMAKE_SOURCE_DIR}/src/protos
)
