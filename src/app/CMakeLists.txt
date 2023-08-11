cmake_minimum_required(VERSION 3.9)
project(abcip)

# Find LibPCAP
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBPCAP REQUIRED libpcap)

# List of source files
set(abcip_SOURCES
    main.cc
    pcap_writer.cc pcap_writer.h
)

# Add the executable target
add_executable(abcip ${abcip_SOURCES})

target_compile_definitions(
	abcip
	PRIVATE
	PACKAGE_NAME="abcip"
	PACKAGE_VERSION="3.0"
	PACKAGE_BUGREPORT="crc181@gmail.com"
)
# Set include directories
target_include_directories(abcip PRIVATE ${CMAKE_SOURCE_DIR}/src/common ${CMAKE_SOURCE_DIR}/src/protos)

# Link against required libraries
target_link_libraries(abcip
    PRIVATE
    proto
    common
    ${LIBPCAP_LIBRARIES}
)

install(TARGETS abcip DESTINATION "${CMAKE_INSTALL_PREFIX}/bin")
