cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(glm VERSION 0.9.9.7 LANGUAGES CXX)

enable_testing()

include_directories("${PROJECT_SOURCE_DIR}")
add_subdirectory(glm)

##################################
# CMake test

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
	add_subdirectory(test)
endif()

##################################
# PkgConfig

set(GLM_VERSION ${PROJECT_VERSION})
configure_file("${CMAKE_SOURCE_DIR}/cmake/glm.pc.in" "${CMAKE_BINARY_DIR}/glm.pc" @ONLY)

##################################
# CMake install script
include(CMakePackageConfigHelpers)

write_basic_package_version_file(
  "${CMAKE_BINARY_DIR}/glm-config-version.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)

include(GNUInstallDirs)

install(TARGETS glm_static glm_shared
	EXPORT glm
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY ${CMAKE_SOURCE_DIR}/glm
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PATTERN "*.cpp" EXCLUDE
	PATTERN "*.txt" EXCLUDE)

install(EXPORT glm
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glm
	NAMESPACE glm::
	FILE "glm-config.cmake")

install(FILES "${CMAKE_BINARY_DIR}/glm.pc"
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig")

install(FILES "${CMAKE_BINARY_DIR}/glm-config-version.cmake"
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/glm")

export(EXPORT glm NAMESPACE glm::)

##################################
# CMake uninstall script

if(NOT TARGET uninstall)
    configure_file(
        "${CMAKE_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
        "${CMAKE_BINARY_DIR}/cmake_uninstall.cmake"
        IMMEDIATE @ONLY)

    add_custom_target(uninstall
        COMMAND ${CMAKE_COMMAND} -P ${CMAKE_BINARY_DIR}/cmake_uninstall.cmake)
endif()
