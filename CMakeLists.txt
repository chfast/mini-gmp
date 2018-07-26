cmake_minimum_required(VERSION 3.5)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

project(mini-gmp VERSION 6.1.2)

add_library(mini-gmp mini-gmp/mini-gmp.c mini-gmp/mini-gmp.h)
target_include_directories(mini-gmp INTERFACE $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

write_basic_package_version_file(mini-gmpConfigVersion.cmake COMPATIBILITY ExactVersion)

configure_package_config_file(cmake/Config.cmake.in mini-gmpConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/mini-gmp)

install(TARGETS mini-gmp EXPORT mini-gmpTargets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR})

install(EXPORT mini-gmpTargets NAMESPACE mini-gmp::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/mini-gmp)

install(FILES mini-gmp/mini-gmp.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/mini-gmpConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/mini-gmpConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/mini-gmp)

if(NOT MSVC)
    target_compile_options(mini-gmp PRIVATE -mtune=generic)
endif()

option(MINIGMP_INSTALL_GMP_HEADER "Install the mini-gmp.h also as gmp.h for compatibility with full GMP")
if(MINIGMP_INSTALL_GMP_HEADER)
    install(FILES mini-gmp/mini-gmp.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} RENAME gmp.h)
endif()
