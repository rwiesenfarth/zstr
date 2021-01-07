
cmake_minimum_required(VERSION 3.6.0)

project(ZStream
  VERSION 0.1.0
  DESCRIPTION "A C++ ZLib wrapper"
)

##  Compiler flags
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

list(APPEND CMAKE_PREFIX_PATH "${PROJECT_SOURCE_DIR}/../../deploy/")

##
##  zlib (use prebuild libraries)
##
find_package(zlib CONFIG REQUIRED)

set(HEADERS
  src/strict_fstream.hpp
  src/zstr.hpp
  src/zstr_make_unique_polyfill.hpp
)


add_library(zstr INTERFACE)

target_include_directories(zstr INTERFACE
  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src>
  $<INSTALL_INTERFACE:include/zstr>
)
target_link_libraries(zstr INTERFACE Zlib::zlibstatic)

install(TARGETS zstr EXPORT zstrConfig)
install(FILES ${HEADERS} DESTINATION include/zstr)

export(TARGETS zstr FILE zstrConfig.cmake NAMESPACE ZStream::)

install(EXPORT zstrConfig DESTINATION share/zstr NAMESPACE ZStream::)
