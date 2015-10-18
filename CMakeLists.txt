cmake_minimum_required (VERSION 2.8)
project (librf24-bcm)
# Some versioning info
set (LIBRF24_VERSION_MAJOR 1)
set (LIBRF24_VERSION_MINOR 0)
set (LIBRF24_VERSION "${LIBRF24_VERSION_MAJOR}.${LIBRF24_VERSION_MINOR}")
# Default buiuls shared libs
set (BUILD_SHARED_LIBS ON)
# Set C11 compiler 
add_definitions (-std=c++11)
# Add module path
set (CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
# Include root directory 
include_directories (${CMAKE_CURRENT_SOURCE_DIR})
# Supported hardware
option (RPI
        "Config for Raspberry 1." OFF) 
option (RPI2
        "Config for Raspberry 2." OFF)
option (CUSTOM
        "Custom hardware config." OFF)
option (BUILD_EXAMPLES
        "Include example applications." OFF)
option (BUILD_EXAMPLES_EXTRA
        "Include example applications." OFF)
# Set correct peripheral address according hardware
if (RPI)
  set (BCM2835_PERI_BASE_ADDRESS 0x20000000)
elseif (RPI2)
  set (BCM2835_PERI_BASE_ADDRESS 0x3F000000)
elseif (CUSTOM)
  if (CUSTOM_PERI_BASE_ADDRESS) 
    set (BCM2835_PERI_BASE_ADDRESS ${CUSTOM_PERI_BASE_ADDRESS})
  else()
    message (FATAL_ERROR "ERROR: Please specify custom peripheral base address using -DCUSTOM_PERI_BASE_ADDRESS=<0x00000000>.")
  endif()
else ()
  message (FATAL_ERROR "ERROR: no peripheral base address could be set.") 
endif () 
# Generate bcm2835 header
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/bcm2835.h.in" 
    "${CMAKE_CURRENT_SOURCE_DIR}/bcm2835.h"
    @ONLY) 
# Generate package config file
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/rf24.pc.in" 
    "${CMAKE_BINARY_DIR}/rf24.pc"
    @ONLY)
# Set the source list
set (LIBRF24_SOURCES  
    RF24.cpp
    bcm2835.c
) 
# Include examples 
if (BUILD_EXAMPLES)
   add_subdirectory (examples)
endif()
# Define targets
add_library (rf24-bcm ${LIBRF24_SOURCES}) 
set_target_properties (rf24-bcm PROPERTIES DEFINE_SYMBOL LIBRF24) 
set_target_properties (rf24-bcm PROPERTIES SOVERSION ${LIBRF24_VERSION}) 
# Installation
install (FILES ${CMAKE_BINARY_DIR}/rf24.pc DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/pkgconfig") 
install (TARGETS rf24-bcm DESTINATION /usr/lib) 
# End CMake
