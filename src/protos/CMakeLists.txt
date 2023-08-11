cmake_minimum_required(VERSION 3.0)
project(proto)

# List of source files
set(libproto_la_SOURCES
    arp.cc arp.h
    eth.cc eth.h
    frag6.cc frag6.h
    gre.cc gre.h
    icmp4.cc icmp4.h
    icmp6.cc icmp6.h
    ip4.cc ip4.h
    ip6.cc ip6.h
    modbus.cc modbus.h
    mpls.cc mpls.h
    opt6.cc opt6.h
    phy.cc phy.h
    pimp.cc pimp.h
    ppp.cc ppp.h
    pppoe.cc pppoe.h
    protocol.cc protocol.h
    prototool.cc prototool.h
    raw.cc raw.h
    rte6.cc rte6.h
    tcp.cc tcp.h
    udp.cc udp.h
    vlan.cc vlan.h
)

# Generate built sources
add_custom_command(
    OUTPUT prototool-inc.h
    COMMAND grep -h -A 2 "__PROTOTOOL_TAG__" ${CMAKE_SOURCE_DIR}/src/protos/*.h | sort -u | grep include | grep -iv xyz > prototool-inc.h
    DEPENDS ${libproto_la_SOURCES}
)

add_custom_command(
    OUTPUT prototool-new.h
    COMMAND grep -h -A 2 "__PROTOTOOL_TAG__" ${CMAKE_SOURCE_DIR}/src/protos/*.h | sort -u | grep PROTOTOOL_NEW | grep -iv xyz > prototool-new.h
    DEPENDS ${libproto_la_SOURCES}
)

# Add the library target
add_library(proto STATIC ${libproto_la_SOURCES} prototool-inc.h prototool-new.h)
target_compile_options(common PRIVATE -fPIC)

# Set include directories
# NOTE Adding CMAKE_BINARY_DIR because of the custom commands above.
target_include_directories(proto PRIVATE ${CMAKE_SOURCE_DIR}/src/common ${CMAKE_BINARY_DIR}/src/protos ${CMAKE_SOURCE_DIR}/src/protos)
