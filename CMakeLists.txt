cmake_minimum_required(VERSION 3.14)

set(PackageProject.cmake_VERSION 13.0.2)

option(${PROJECT_NAME}_INSTALL "Install the script for local system?" NO)

set(PACKAGE_PROJECT_ROOT_PATH_DEFAULT_VALUE "\$\{CMAKE_CURRENT_LIST_DIR\}/PackageProjectFiles")

set(PACKAGE_PROJECT_ROOT_PATH
    "${PACKAGE_PROJECT_ROOT_PATH_DEFAULT_VALUE}"
    CACHE
      PATH
      "The path to the PackageProject directory in which will be Config.cmake.in and version.h.in"
)

configure_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfig.cmake.in
  ${CMAKE_CURRENT_LIST_DIR}/cmake/PackageProject.cmake/PackageProject.cmakeConfig.cmake @ONLY
)

if(${PackageProject.cmake_INSTALL})
  include(GNUInstallDirs)

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
