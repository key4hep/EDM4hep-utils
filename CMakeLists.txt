cmake_minimum_required(VERSION 3.3)

project(EDM4HEPUtils)

# project version
SET(${PROJECT_NAME}_VERSION_MAJOR 0)
SET(${PROJECT_NAME}_VERSION_MINOR 0)
SET(${PROJECT_NAME}_VERSION_PATCH 0)

SET(${PROJECT_NAME}_VERSION "${${PROJECT_NAME}_VERSION_MAJOR}.${${PROJECT_NAME}_VERSION_MINOR}.${${PROJECT_NAME}_VERSION_PATCH}")

#--- Definitely need EDM4HEP data model library
find_package(EDM4HEP)

#--- Define basic build settings -----------------------------------------------
# - Use GNU-style hierarchy for installing build products
include(GNUInstallDirs)

if (${APPLE})
    set(CPP_STANDARD_FLAGS "-std=c++${CMAKE_CXX_STANDARD}\ -stdlib=libc++")
endif()
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -DDROP_CGAL ${CPP_STANDARD_FLAGS} -Wall -Wextra -Wpedantic -Wno-unused-variable -Wno-unused-parameter")

# - Define a default build type when using a single-mode tool like make/ninja
# If you're using a build tool that supports multiple modes (Visual Studio,
# Xcode), this setting has no effect.
# HSF recommend RelWithDebInfo (optimized with debugging symbols) as this is
# generally the mode used by system packaging (rpm, deb, spack, macports).
# However, it can be overriden by passing ``-DCMAKE_BUILD_TYPE=<type>`` when
# invoking CMake
if(NOT CMAKE_CONFIGURATION_TYPES)
  if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE RelWithDebInfo
      CACHE STRING "Choose the type of build, options are: None Release MinSizeRel Debug RelWithDebInfo"
      FORCE
      )
  else()
    set(CMAKE_BUILD_TYPE "${CMAKE_BUILD_TYPE}"
      CACHE STRING "Choose the type of build, options are: None Release MinSizeRel Debug RelWithDebInfo"
      FORCE
      )
  endif()
endif()

set(CMAKE_CXX_STANDARD 17 CACHE STRING "")
if (NOT CMAKE_CXX_STANDARD MATCHES "17")
  message(FATAL_ERROR "Unsupported C++ standard: ${CMAKE_CXX_STANDARD}")
endif()

#--- enable unit testing capabilities ------------------------------------------
include(CTest)

#--- build Doxygen documentation -----------------------------------------------
option(BUILD_DOCUMENTATION "Generate doxygen documentation target." OFF)
if(BUILD_DOCUMENTATION)
  include(cmake/EDM4HEPUtilsDoxygen.cmake)
endif()


#--- add version files ---------------------------------------------------------
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/EDM4HEPUtilsVersion.h ${CMAKE_CURRENT_BINARY_DIR}/EDM4HEPUtilsVersion.h)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/EDM4HEPUtilsVersion.h
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

#--- add license files ---------------------------------------------------------
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/LICENSE
              ${CMAKE_CURRENT_SOURCE_DIR}/NOTICE
              DESTINATION ${CMAKE_INSTALL_DOCDIR})

#--- add CMake infrastructure
include(cmake/EDM4HEPUtilsCreateConfig.cmake)
