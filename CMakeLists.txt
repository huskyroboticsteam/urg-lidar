cmake_minimum_required(VERSION 3.5)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

project(URGLidar
  LANGUAGES C CXX
  VERSION 1.2.5.3)

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
install(DIRECTORY include/c/
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/urg_c)
install(DIRECTORY include/cpp/
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/urg_cpp)

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

# Borrowed/adapted from https://www.scivision.dev/cmake-cpack-basic/
set(CPACK_GENERATOR "DEB")
set(CPACK_PACKAGE_NAME "urg-lidar")
# Version format: major.minor.patch-tweak
set(CPACK_PACKAGE_VERSION "${CMAKE_PROJECT_VERSION_MAJOR}.${CMAKE_PROJECT_VERSION_MINOR}.${CMAKE_PROJECT_VERSION_PATCH}-${CMAKE_PROJECT_VERSION_TWEAK}")
set(CPACK_PACKAGE_FILE_NAME ${CPACK_PACKAGE_NAME}-${CMAKE_SYSTEM_PROCESSOR}-${CPACK_PACKAGE_VERSION})
set(CPACK_PACKAGE_CONTACT "Husky Robotics Team <uwrobots@uw.edu>")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE.txt")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
set(CPACK_PACKAGE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

# not .gitignore as its regex syntax is distinct
file(READ ${CMAKE_CURRENT_LIST_DIR}/cmake/.cpack_ignore _cpack_ignore)
string(REGEX REPLACE "\n" ";" _cpack_ignore ${_cpack_ignore})
set(CPACK_SOURCE_IGNORE_FILES "${_cpack_ignore}")

install(FILES ${CPACK_RESOURCE_FILE_README} ${CPACK_RESOURCE_FILE_LICENSE}
  DESTINATION share/docs/${PROJECT_NAME})

include(CPack)

endif()
