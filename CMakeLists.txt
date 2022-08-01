cmake_minimum_required(VERSION 3.8)

project (vcpkgdemo)

message(STATUS "This is BINARY dir " ${CMAKE_BINARY_DIR})
message(STATUS "This is SOURCE dir " ${CMAKE_SOURCE_DIR})

add_subdirectory(lib)
add_subdirectory(src bin)

# ----------------------------------------------------------------------------
# package information
set (CPACK_PACKAGE_VERSION_MAJOR    1)
set (CPACK_PACKAGE_VERSION_MINOR    2)
set (CPACK_PACKAGE_VERSION_PATCH    3)
set (CPACK_PACKAGE_CONTACT          "zieckey@gmail.com")
set (CPACK_GENERATOR                "TGZ")
set (CPACK_PACKAGING_INSTALL_PREFIX "/home/weizili/hello")

include (CPack)

# Install the target and create export-set
install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}Targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        PUBLIC_HEADER DESTINATION include)

# Generate the version file for the config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        VERSION ${PACKAGE_VERSION}
        COMPATIBILITY SameMajorVersion)

# Exporting Targets from the Build Tree
install(EXPORT ${PROJECT_NAME}Targets
        DESTINATION "lib/cmake/${PROJECT_NAME}")

# Create config file
configure_package_config_file(
        ${CMAKE_SOURCE_DIR}/Config.cmake.in ${PROJECT_NAME}Config.cmake
        INSTALL_DESTINATION "lib/cmake/${PROJECT_NAME}")

# Install config files
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        DESTINATION "lib/cmake/${PROJECT_NAME}")

