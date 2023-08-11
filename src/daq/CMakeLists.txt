cmake_minimum_required(VERSION 3.9)
project(daq_abc)

# Find LibDAQ
find_package(PkgConfig REQUIRED)
pkg_search_module(LIBDAQ REQUIRED libdaq>=3.0.11)

# Add the library target
add_library(daq_abc MODULE
    ${CMAKE_SOURCE_DIR}/src/protos/prototool-inc.h
    ${CMAKE_SOURCE_DIR}/src/protos/prototool-new.h
)

target_compile_options(daq_abc PRIVATE -fPIC)

set_target_properties(
    daq_abc
    PROPERTIES
    PREFIX ""
)

# FIXME: The following static linking does not work when building the
# `daq_abc.so` module but does work in the `abcip` executable.
#
# target_link_libraries(
#    daq_abc
#    PRIVATE
#    ${CMAKE_BINARY_DIR}/src/common/libcommon.a
#    ${CMAKE_BINARY_DIR}/src/protos/libproto.a
#)

# Hack around static library issue
set(libcommon_SOURCES
    ${CMAKE_SOURCE_DIR}/src/common/abc_io.cc
    ${CMAKE_SOURCE_DIR}/src/common/abc_io.h
    ${CMAKE_SOURCE_DIR}/src/common/abc_ip.cc
    ${CMAKE_SOURCE_DIR}/src/common/abc_ip.h
    ${CMAKE_SOURCE_DIR}/src/common/attribute.cc
    ${CMAKE_SOURCE_DIR}/src/common/attribute.h
    ${CMAKE_SOURCE_DIR}/src/common/cake.cc
    ${CMAKE_SOURCE_DIR}/src/common/cake.h
    ${CMAKE_SOURCE_DIR}/src/common/cmd_parser.cc
    ${CMAKE_SOURCE_DIR}/src/common/cmd_parser.h
    ${CMAKE_SOURCE_DIR}/src/common/command.h
    ${CMAKE_SOURCE_DIR}/src/common/data.cc
    ${CMAKE_SOURCE_DIR}/src/common/data.h
    ${CMAKE_SOURCE_DIR}/src/common/data_parser.cc
    ${CMAKE_SOURCE_DIR}/src/common/data_parser.h
    ${CMAKE_SOURCE_DIR}/src/common/dumb_user.cc
    ${CMAKE_SOURCE_DIR}/src/common/dumb_user.h
    ${CMAKE_SOURCE_DIR}/src/common/field.cc
    ${CMAKE_SOURCE_DIR}/src/common/field.h
    ${CMAKE_SOURCE_DIR}/src/common/packet.h
    ${CMAKE_SOURCE_DIR}/src/common/parser.h
    ${CMAKE_SOURCE_DIR}/src/common/pile.cc
    ${CMAKE_SOURCE_DIR}/src/common/pile.h
    ${CMAKE_SOURCE_DIR}/src/common/pseudo_hdr.cc
    ${CMAKE_SOURCE_DIR}/src/common/pseudo_hdr.h
    ${CMAKE_SOURCE_DIR}/src/common/reader.h
    ${CMAKE_SOURCE_DIR}/src/common/status.cc
    ${CMAKE_SOURCE_DIR}/src/common/status.h
    ${CMAKE_SOURCE_DIR}/src/common/stream_reader.cc
    ${CMAKE_SOURCE_DIR}/src/common/stream_reader.h
    ${CMAKE_SOURCE_DIR}/src/common/user.cc
    ${CMAKE_SOURCE_DIR}/src/common/user.h
    ${CMAKE_SOURCE_DIR}/src/common/writer.h
)

set(libproto_SOURCES
    ${CMAKE_SOURCE_DIR}/src/protos/arp.cc
    ${CMAKE_SOURCE_DIR}/src/protos/arp.h
    ${CMAKE_SOURCE_DIR}/src/protos/eth.cc
    ${CMAKE_SOURCE_DIR}/src/protos/eth.h
    ${CMAKE_SOURCE_DIR}/src/protos/frag6.cc
    ${CMAKE_SOURCE_DIR}/src/protos/frag6.h
    ${CMAKE_SOURCE_DIR}/src/protos/gre.cc
    ${CMAKE_SOURCE_DIR}/src/protos/gre.h
    ${CMAKE_SOURCE_DIR}/src/protos/icmp4.cc
    ${CMAKE_SOURCE_DIR}/src/protos/icmp4.h
    ${CMAKE_SOURCE_DIR}/src/protos/icmp6.cc
    ${CMAKE_SOURCE_DIR}/src/protos/icmp6.h
    ${CMAKE_SOURCE_DIR}/src/protos/ip4.cc
    ${CMAKE_SOURCE_DIR}/src/protos/ip4.h
    ${CMAKE_SOURCE_DIR}/src/protos/ip6.cc
    ${CMAKE_SOURCE_DIR}/src/protos/ip6.h
    ${CMAKE_SOURCE_DIR}/src/protos/modbus.cc
    ${CMAKE_SOURCE_DIR}/src/protos/modbus.h
    ${CMAKE_SOURCE_DIR}/src/protos/mpls.cc
    ${CMAKE_SOURCE_DIR}/src/protos/mpls.h
    ${CMAKE_SOURCE_DIR}/src/protos/opt6.cc
    ${CMAKE_SOURCE_DIR}/src/protos/opt6.h
    ${CMAKE_SOURCE_DIR}/src/protos/phy.cc
    ${CMAKE_SOURCE_DIR}/src/protos/phy.h
    ${CMAKE_SOURCE_DIR}/src/protos/pimp.cc
    ${CMAKE_SOURCE_DIR}/src/protos/pimp.h
    ${CMAKE_SOURCE_DIR}/src/protos/ppp.cc
    ${CMAKE_SOURCE_DIR}/src/protos/ppp.h
    ${CMAKE_SOURCE_DIR}/src/protos/pppoe.cc
    ${CMAKE_SOURCE_DIR}/src/protos/pppoe.h
    ${CMAKE_SOURCE_DIR}/src/protos/protocol.cc
    ${CMAKE_SOURCE_DIR}/src/protos/protocol.h
    ${CMAKE_SOURCE_DIR}/src/protos/prototool-inc.h
    ${CMAKE_SOURCE_DIR}/src/protos/prototool-new.h
    ${CMAKE_SOURCE_DIR}/src/protos/prototool.cc
    ${CMAKE_SOURCE_DIR}/src/protos/prototool.h
    ${CMAKE_SOURCE_DIR}/src/protos/raw.cc
    ${CMAKE_SOURCE_DIR}/src/protos/raw.h
    ${CMAKE_SOURCE_DIR}/src/protos/rte6.cc
    ${CMAKE_SOURCE_DIR}/src/protos/rte6.h
    ${CMAKE_SOURCE_DIR}/src/protos/tcp.cc
    ${CMAKE_SOURCE_DIR}/src/protos/tcp.h
    ${CMAKE_SOURCE_DIR}/src/protos/udp.cc
    ${CMAKE_SOURCE_DIR}/src/protos/udp.h
    ${CMAKE_SOURCE_DIR}/src/protos/vlan.cc
    ${CMAKE_SOURCE_DIR}/src/protos/vlan.h
)

# List of source files
target_sources(
    daq_abc
    PRIVATE
    ${libcommon_SOURCES}
    ${libproto_SOURCES}
    abc_daq.cc
    abc_daq.h
    base_daq.cc
    base_daq.h
    daq_lib.cc
    daq_lib.h
    daq_writer.cc
    daq_writer.h
)

target_include_directories(
    daq_abc
    PRIVATE
    ${CMAKE_SOURCE_DIR}/src/common
    ${CMAKE_SOURCE_DIR}/src/protos
)

install(TARGETS daq_abc DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/daq")


