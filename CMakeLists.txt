
cmake_minimum_required(VERSION 3.6.0)

project(ZStream
  VERSION 0.1.0
  DESCRIPTION "A C++ ZLib wrapper"
)

##  Compiler flags
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

##
##  OpenMVG (use prebuild libraries)
##
find_package(OpenMVG CONFIG REQUIRED PATHS ${CMAKE_INSTALL_PREFIX}/share/openMVG/cmake)

set(HEADERS
  src/strict_fstream.hpp
  src/zstr.hpp
)


add_library(zstr INTERFACE)

target_include_directories(zstr INTERFACE
  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src>
  $<INSTALL_INTERFACE:include/zstr>
  $<BUILD_INTERFACE:${CMAKE_INSTALL_PREFIX}/include/openMVG/third_party/zlib>
  $<INSTALL_INTERFACE:include/openMVG/third_party/zlib>
)
target_link_libraries(zstr INTERFACE "zlib")

install(TARGETS zstr EXPORT zstrConfig)
install(FILES ${HEADERS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/zstr)

export(TARGETS zstr FILE zstrConfig.cmake)

install(EXPORT zstrConfig DESTINATION share/zstr)
