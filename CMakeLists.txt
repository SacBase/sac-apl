CMAKE_MINIMUM_REQUIRED (VERSION 3.20)

PROJECT (sac-apl)

# Where the compiled sac modules result
SET (DLL_BUILD_DIR "${PROJECT_BINARY_DIR}/lib")

# For what targets we build modules
SET (TARGETS    seq seq_checks mt_pth CACHE STRING "Build for these targets")
SET (SAC2C_EXEC                       CACHE STRING "Path to sac2c compiler")

# Check whether sac2c is operational
LIST (APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake-common")
INCLUDE ("cmake-common/check-sac2c.cmake")
INCLUDE ("cmake-common/misc-macros.cmake")

# For every target run CMakeLists.txt in src
FOREACH (TARGET IN ITEMS ${TARGETS})
    ADD_SUBDIRECTORY (src src-${TARGET})
ENDFOREACH ()

# This build target is responsible for generating the package sac2crc file
CREATE_SAC2CRC_TARGET ("apl" "${DLL_BUILD_DIR}" "${DLL_BUILD_DIR}" "")
