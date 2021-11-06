cmake_minimum_required(VERSION 3.5)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

project(URGLidar
  LANGUAGES C CXX
  VERSION 1.2.5)

set(C_FILES
  src/urg_connection.c
  src/urg_debug.c
  src/urg_ring_buffer.c
  src/urg_sensor.c
  src/urg_serial.c
  src/urg_serial_utils.c
  src/urg_tcpclient.c
  src/urg_utils.c)

set(CXX_FILES
  src/Urg_driver.cpp
  src/ticks.cpp)

add_library(urg_c STATIC ${C_FILES})
add_library(urg_cpp STATIC ${CXX_FILES})
target_include_directories(urg_c
  PUBLIC
  $<INSTALL_INTERFACE:include>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include/c>)
target_include_directories(urg_cpp
  PUBLIC
  $<INSTALL_INTERFACE:include>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include/c>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include/cpp>)

if (DEFINED PROJECT_NAME)
install(TARGETS urg_c urg_cpp
  EXPORT urg-targets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(EXPORT urg-targets
  FILE URGLidarTargets.cmake
  NAMESPACE URGLidar::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/URGLidar)

configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/URGLidarConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/URGLidarConfig.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/URGLidar)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/URGLidarConfig.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/URGLidar)

endif()
