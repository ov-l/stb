cmake_minimum_required(VERSION 3.21)
project(stb LANGUAGES C CXX VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 20)

include(GNUInstallDirs)


##
## CONFIGURATION
##
set(${PROJECT_NAME}_TARGET_NAME       ${PROJECT_NAME})
set(${PROJECT_NAME}_INCLUDE_BUILD_DIR "${PROJECT_SOURCE_DIR}")

include(GNUInstallDirs)

##
## TARGET
##
message("Building: ${${PROJECT_NAME}_TARGET_NAME}")
add_library(${${PROJECT_NAME}_TARGET_NAME} INTERFACE)
add_library(${PROJECT_NAME}::${${PROJECT_NAME}_TARGET_NAME} ALIAS ${${PROJECT_NAME}_TARGET_NAME})

target_include_directories(
    ${${PROJECT_NAME}_TARGET_NAME}
    INTERFACE $<BUILD_INTERFACE:${${PROJECT_NAME}_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

##
## INSTALL
##
install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}_Targets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

include(CMakePackageConfigHelpers)
write_basic_package_version_file("${PROJECT_NAME}ConfigVersion.cmake"
                                 VERSION ${PROJECT_VERSION}
                                 COMPATIBILITY SameMajorVersion)

configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}-config.cmake.in"
    "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
    INSTALL_DESTINATION
      cmake
)


 install(EXPORT ${PROJECT_NAME}_Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION cmake)

install(FILES 
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
  DESTINATION cmake)

install(FILES ${CMAKE_SOURCE_DIR}/stb_image.h ${CMAKE_SOURCE_DIR}/stb_image_resize2.h ${CMAKE_SOURCE_DIR}/stb_image_write.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})