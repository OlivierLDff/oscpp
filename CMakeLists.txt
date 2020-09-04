#
#   OSCPP CMake script
#

cmake_minimum_required(VERSION 3.14.0 FATAL_ERROR)

set(OSCPP_TARGET "oscpp" CACHE STRING "oscpp target name")
set(OSCPP_ENABLE_TESTS OFF CACHE BOOL "Enable oscpp tests")

project(${OSCPP_TARGET} VERSION "1.0.0" LANGUAGES CXX)

add_library(${OSCPP_TARGET} INTERFACE)
set(OSCPP_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include")
set(OSCPP_PRIVATE_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include/oscpp")

set(OSCPP_SRCS
  "${OSCPP_PRIVATE_INCLUDE_DIR}/client.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/error.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/print.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/server.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/types.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/util.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/detail/host.hpp"
  "${OSCPP_PRIVATE_INCLUDE_DIR}/detail/stream.hpp"
  )

foreach(SRC_FILE ${OSCPP_SRCS})
    target_sources(${OSCPP_TARGET} INTERFACE ${SRC_FILE})
endforeach()

source_group("Oscpp" FILES ${OSCPP_SRCS})

target_include_directories(${OSCPP_TARGET} INTERFACE ${OSCPP_INCLUDE_DIR})

include(TestBigEndian)
test_big_endian(IS_BIG_ENDIAN)
if(IS_BIG_ENDIAN)
  target_compile_definitions(${OSCPP_TARGET} INTERFACE -DOSCPP_BIG_ENDIAN)
else()
  target_compile_definitions(${OSCPP_TARGET} INTERFACE -DOSCPP_LITTLE_ENDIAN)
endif()
target_compile_features(${OSCPP_TARGET} INTERFACE cxx_std_11)

if(OSCPP_ENABLE_TESTS)
  enable_testing()
  add_subdirectory(test)
endif()