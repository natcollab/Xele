#[===============================================================================[.rst
# CTPMini/CMakeLists.txt
#
# CTPMini library.
# ^^^^^^^^^^^^
#
# Imported targets in global scope:
#
#   CTPMini::CTPMini
#
#]===============================================================================]

# Setup minimum required version of cmake, policy
# This command should be the first command in the project
cmake_minimum_required(VERSION 3.15...3.17)

# Setup Project name and version
# CMakeTest_VERSION and PROJECT_VERSION variables are set
project(CTPMini VERSION 1.0
  DESCRIPTION "Interface to CTPMini library"
  LANGUAGES CXX
  )

#=================================================================================

# Target: CTPMini
add_library(CTPMini INTERFACE)
add_library(CTPMini::CTPMini ALIAS CTPMini)

set(CTPMini_INCLUDE_INSTALL_DIR include/CTPMini)
set(CTPMini_LIB_INSTALL_DIR lib/CTPMini)
set(CTPMini_CMAKE_INSTALL_DIR lib/cmake/CTPMini)

target_include_directories(CTPMini
  INTERFACE  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
             $<INSTALL_INTERFACE:${CTPMini_INCLUDE_INSTALL_DIR}>
  )

target_link_libraries(CTPMini
  INTERFACE  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/libthostmduserapi.so>
             $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/libthosttraderapi.so>
             $<INSTALL_INTERFACE:${CTPMini_LIB_INSTALL_DIR}/libthostmduserapi.so>
             $<INSTALL_INTERFACE:${CTPMini_LIB_INSTALL_DIR}/libthosttraderapi.so>
  )

#=================================================================================

# Install : CTPMini
install(DIRECTORY include/
  DESTINATION ${CTPMini_INCLUDE_INSTALL_DIR}
  )

install(DIRECTORY lib/
  DESTINATION ${CTPMini_LIB_INSTALL_DIR}
  )

#=================================================================================

# Install export targets

install(TARGETS CTPMini EXPORT CTPMini_export)

# TODO: library path in export target file is relative path, which is not correct
install(EXPORT CTPMini_export
  NAMESPACE CTPMini::
  FILE CTPMiniTargets.cmake
  DESTINATION ${CTPMini_CMAKE_INSTALL_DIR}
  )

#=================================================================================

# Install package config and version files

include(CMakePackageConfigHelpers)

# Package version file
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/CTPMiniConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion
  )

# Package config file
configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/CTPMiniConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/CTPMiniConfig.cmake
  INSTALL_DESTINATION ${CTPMini_CMAKE_INSTALL_DIR}
  PATH_VARS
    CTPMini_INCLUDE_INSTALL_DIR
    CTPMini_LIB_INSTALL_DIR
  )

# Install the config and version files
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/CTPMiniConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/CTPMiniConfigVersion.cmake
  DESTINATION ${CTPMini_CMAKE_INSTALL_DIR}
  )
