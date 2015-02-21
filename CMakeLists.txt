#####################################################################
# Section 1: Set version, project name, build type, compiler flags, etc
#####################################################################

# Check the cmake version and name the project
cmake_minimum_required (VERSION 2.8)
# The project name may be the same as the git module, or it may be different
project (lmdb)

# Set a version - This should be updated by the developer as appropriate.
set (PACKAGE_MAJOR_VERSION 0)
set (PACKAGE_MINOR_VERSION 9)
set (PACKAGE_PATCH_VERSION 11)

set (${PROJECT_NAME}_MAJOR_VERSION ${PACKAGE_MAJOR_VERSION})
set (${PROJECT_NAME}_MINOR_VERSION ${PACKAGE_MINOR_VERSION})
set (${PROJECT_NAME}_PATCH_VERSION ${PACKAGE_PATCH_VERSION})
set (PACKAGE_VERSION
  ${PACKAGE_MAJOR_VERSION}.${PACKAGE_MINOR_VERSION}.${PACKAGE_PATCH_VERSION})
set (${PROJECT_NAME}_VERSION ${PACKAGE_VERSION})
string(TOLOWER ${PROJECT_NAME} PROJECT_NAME_LOWER)

# Set the find_package search path and the installation directory.
#
#   If PACKAGE_PATH is defined, it is prepended to CMAKE_PREFIX_PATH.
#   On Windows, specifying PACKAGE_PATH is mandatory.
#
#   If INSTALL_PATH is not specified but PACKAGE_PATH is, then
#   INSTALL_PATH is set to
#     ${PACKAGE_PATH}, if Linux (not Windows)
#     ${PACKAGE_PATH}/${PROJECT_NAME}-${PACKAGE_VERSION}, if Windows
#
#   If INSTALL_PATH is now defined, then set CMAKE_INSTALL_PREFIX to
#   INSTALL_PATH.
#
#   Typically, only PACKAGE_PATH needs to be specified, eg.,
#   cmake -D PACKAGE_PATH=/opt .. (on Linux)
#     => CMAKE_PREFIX_PATH=/opt   CMAKE_INSTALL_PREFIX=/opt
#   cmake -D PACKAGE_PATH=c:/pkg .. (on Windows)
#     => CMAKE_PREFIX_PATH=c:/pkg CMAKE_INSTALL_PREFIX=c:/pkg/tnt-1.2.6

if (DEFINED PACKAGE_PATH)
  set (CMAKE_PREFIX_PATH ${PACKAGE_PATH} ${CMAKE_PREFIX_PATH})
  message (STATUS "CMAKE_PREFIX_PATH set to ${CMAKE_PREFIX_PATH}")
elseif (WIN32)
  # If you need to support the traditional cmake mechanisms, omit this check.
  message (FATAL_ERROR
    "You MUST set PACKAGE_PATH, e.g. cmake -D PACKAGE_PATH=C:/pkg")
endif ()

if (NOT DEFINED INSTALL_PATH AND DEFINED PACKAGE_PATH)
  if (NOT WIN32)
    set (INSTALL_PATH ${PACKAGE_PATH} CACHE PATH "Install Directory" FORCE)
  else ()
    set (INSTALL_PATH ${PACKAGE_PATH}/${PROJECT_NAME}-${PACKAGE_VERSION}
    CACHE PATH "Install Directory" FORCE)
  endif ()
endif ()
if (DEFINED INSTALL_PATH)
  file (TO_CMAKE_PATH ${INSTALL_PATH} CMAKE_INSTALL_PREFIX)
endif ()
message (STATUS "CMAKE_INSTALL_PREFIX set to ${CMAKE_INSTALL_PREFIX}")

# Set a default build type ("Release") if no build type is set.
# This is important for Linux, but not for Visual Studio users, since they
# can choose in the IDE.
if (NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
   set (CMAKE_BUILD_TYPE Release)
endif ()

# The debug version of the library will be called VisionLib_d.
set (CMAKE_DEBUG_POSTFIX _d)

##############################################################################
# Section 2: External Dependencies

#find_package(Boost)

##############################################################################
# Section 3: Internal Dependencies
##############################################################################

##############################################################################
# Section 4: Includes
##############################################################################

# Set the include directories
include_directories (${CMAKE_CURRENT_SOURCE_DIR})

 


##############################################################################
# Section 6: Recurse to subdirectories
##############################################################################


# Recurse into subdirectories and run those CMakeLists.txt
add_subdirectory (libraries/liblmdb)

# List of libraries built in this project
set(PROJECT_LIBRARY_LIST lmdb)

#############################################################################
# Config file generation and installation
#############################################################################
add_subdirectory (cmake)
