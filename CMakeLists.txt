cmake_minimum_required(VERSION 3.10)
set(RAISIM_VERSION 1.0.0)
project(raisim VERSION ${RAISIM_VERSION} LANGUAGES CXX)
set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

# Include CMake helper modules
include(GNUInstallDirs)

# Build options
option(RAISIM_EXAMPLE "Build example applications." TRUE)
option(RAISIM_MATLAB "Build a matlab wrapper." FALSE)
option(RAISIM_PY "Build a python wrapper." FALSE)
option(RAISIM_DOC "Build a python wrapper." FALSE)


if (UNIX AND NOT APPLE AND NOT ANDROID AND NOT WEBGL AND NOT WIN32)
    set(RAISIM_OS linux)  
    list(APPEND CMAKE_PREFIX_PATH ${CMAKE_CURRENT_SOURCE_DIR}/raisim/${RAISIM_OS})
elseif(APPLE)
    set(RAISIM_OS mac)
    list(APPEND CMAKE_PREFIX_PATH ${CMAKE_CURRENT_SOURCE_DIR}/raisim/${RAISIM_OS})
elseif(WIN32)
    set(RAISIM_OS win32)
    list(APPEND CMAKE_PREFIX_PATH ${CMAKE_CURRENT_SOURCE_DIR}/raisim/${RAISIM_OS}/mt_release)
    list(APPEND CMAKE_PREFIX_PATH ${CMAKE_CURRENT_SOURCE_DIR}/raisim/${RAISIM_OS}/mt_debug)
endif()

#==
# Package installation
#==
install(
        DIRECTORY
        ${CMAKE_CURRENT_LIST_DIR}/raisim/${RAISIM_OS}/include
        DESTINATION
        ${CMAKE_INSTALL_PREFIX}
)

install(
        DIRECTORY
        "${CMAKE_CURRENT_LIST_DIR}/raisim/${RAISIM_OS}/lib"
        DESTINATION
        ${CMAKE_INSTALL_PREFIX}
)

if(RAISIM_ALL)
    set(RAISIM_EXAMPLE TRUE)
    set(RAISIM_MATLAB TRUE)
    set(RAISIM_PY TRUE)
    set(RAISIM_DOC TRUE)
endif()

if(RAISIM_EXAMPLE)
    add_subdirectory(examples)
endif()

if(RAISIM_MATLAB)
    add_subdirectory(raisimMatlab)
endif()

if(RAISIM_PY)
    add_subdirectory(thirdParty/pybind11)
    add_subdirectory(raisimPy)
endif()

if(RAISIM_DOC)
    add_subdirectory(docs)
endif()

# EOF
