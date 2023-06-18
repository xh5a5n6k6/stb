# CMake script to build stb header-only library.
#
# Reference:
#   - https://dominikberner.ch/cmake-interface-lib/
#   - https://github.com/TzuChieh/stb/blob/photon/CMakeLists.txt

# Setting CMake minimum version to 3.21 to align with Cadise Renderer's requirement.
cmake_minimum_required(VERSION 3.21)

project(stb 
    VERSION 0.0.1
    DESCRIPTION "single-file public domain (or MIT licensed) libraries for C/C++"
    HOMEPAGE_URL "https://github.com/nothings/stb")

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(${PROJECT_NAME} INTERFACE)

target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>/)

target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_20)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}_internal_exported_targets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

# Package related setup
write_basic_package_version_file(
    "${PROJECT_NAME}ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY ExactVersion)

configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in"
    "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
    INSTALL_DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/cmake/)

install(EXPORT ${PROJECT_NAME}_internal_exported_targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/cmake/)

install(
    FILES
        "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
        "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/cmake/)

install(
    FILES
        "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
        "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/cmake/)

# Install source files to exported destination.
install(DIRECTORY ${PROJECT_SOURCE_DIR}/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/
    FILES_MATCHING PATTERN "*.h"
    PATTERN ".git" EXCLUDE
    PATTERN ".github" EXCLUDE
    PATTERN "cmake" EXCLUDE
    PATTERN "data" EXCLUDE
    PATTERN "deprecated" EXCLUDE
    PATTERN "docs" EXCLUDE
    PATTERN "tests" EXCLUDE
    PATTERN "tools" EXCLUDE)