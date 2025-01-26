cmake_minimum_required(VERSION 3.14)

project(PackageProject.cmake VERSION 13.0.5)

option(${PROJECT_NAME}_INSTALL "Install the script for local system?" NO)

set(PACKAGE_PROJECT_ROOT_PATH_DEFAULT_VALUE "\$\{CMAKE_CURRENT_LIST_DIR\}/PackageProjectFiles")

set(PACKAGE_PROJECT_ROOT_PATH
    "${PACKAGE_PROJECT_ROOT_PATH_DEFAULT_VALUE}"
    CACHE
      PATH
      "The path to the PackageProject directory in which will be Config.cmake.in and version.h.in"
)

include(CMakePackageConfigHelpers)

include(GNUInstallDirs)

configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfig.cmake.in
  ${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfig.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/cmake/PackageProject.cmake
)

write_basic_package_version_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfigVersion.cmake
  VERSION ${PackageProject.cmake_VERSION}
  COMPATIBILITY AnyNewerVersion ARCH_INDEPENDENT
)

if(${PackageProject.cmake_INSTALL})

  set(CMAKEDIR
      ${CMAKE_INSTALL_DATADIR}/cmake/PackageProject.cmake/
      CACHE
        PATH
        "Path to a folder to which install the script PackageProjectConfig.cmake . By default: ${CMAKE_INSTALL_DATADIR}/cmake/PackageProject.cmake/"
  )

  install(
    FILES "${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfig.cmake"
    DESTINATION "${CMAKEDIR}"
  )

  install(
    FILES
      "${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfigVersion.cmake"
    DESTINATION "${CMAKEDIR}"
  )
  if("${PACKAGE_PROJECT_ROOT_PATH_DEFAULT_VALUE}" STREQUAL "${PACKAGE_PROJECT_ROOT_PATH}")
    install(DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProjectFiles"
            DESTINATION "${CMAKEDIR}"
    )
  else()
    install(DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProjectFiles"
            DESTINATION "${PACKAGE_PROJECT_ROOT_PATH}/../"
    )
  endif()

  message(STATUS "Use `find_package(PackageProject.cmake)` to use it.")
endif()

include(${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfig.cmake)
