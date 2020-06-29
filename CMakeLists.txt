cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
### Add our cmake util path
list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_SOURCE_DIR}/cmake)

### Get the version from the current git tag.
find_package(Git)
if(Git_FOUND)
    include(MunkeiVersionFromGit)
    version_from_git(
            GIT_EXECUTABLE ${GIT_EXECUTABLE}
    )
endif()

project(Blink1-Tools
        VERSION ${VERSION}
        DESCRIPTION "A library for controlling Blink1 devices."
        LANGUAGES C)

### Additional Cmake features.
include(CMakePackageConfigHelpers)
include(CMakeParseArguments)
include(CMakePrintHelpers)
#include(CMakePrintSystemInformation)
include(CPack)
include(GNUInstallDirs)
include(FeatureSummary)

### Setup pretty print.
include(colors)
define_colors()

find_package(PkgConfig)

### Do stuff for non-Darwin.
if (NOT ${CMAKE_HOST_SYSTEM_NAME} MATCHES "Darwin")
    cmake_print_variables(CMAKE_HOST_SYSTEM_NAME)
    message(FATAL_ERROR "${CMAKE_HOST_SYSTEM_NAME} not supported.")
    #    set(CMAKE_TOOLCHAIN_FILE  "${CMAKE_SOURCE_DIR}/Toolchain-aarch64-himix100-linux.cmake")
endif ()

if(GIT_FOUND AND (EXISTS "${PROJECT_SOURCE_DIR}/.git" OR EXISTS "${PROJECT_SOURCE_DIR}/.gitmodules")) # Update submodules as needed
    option(GIT_SUBMODULE "Check submodules during build" ON)
    set(GIT_SHALLOW ON)
    message(STATUS "GIT_EXECUTABLE is ${GIT_EXECUTABLE}")
    if(GIT_SUBMODULE)
        message(STATUS "Submodule update")
        execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                RESULT_VARIABLE git_result
                COMMAND_ECHO STDOUT)
        if(NOT ${git_result} EQUAL "0")
            message(FATAL_ERROR "git submodule update --init failed with ${git_result}, please checkout submodules")
        endif()
    endif()
else()
    message(FATAL "Git not found.")
endif()


set(GLOBAL CMAKE_C_STANDARD 11
    CMAKE_C_EXTENSIONS OFF
    CMAKE_C_STANDARD_REQUIRED ON)

# Support folders in IDE's
set_property(DIRECTORY PROPERTY USE_FOLDERS ON)

### Require out-of-source builds
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file).
    Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()



include_directories(.)
include_directories(blink1-mini-tool)
include_directories(server)
include_directories(server/mongoose)

add_executable(blink1_tool
#        blink1-mini-tool/blink1-mini-tool.c
#        blink1-mini-tool/hiddata.c
#        blink1-mini-tool/hiddata.h
#        blink1control-tool/blink1control-tool.c
#        blink1raw/blink1raw.c
#        server/mongoose/mongoose.c
#        server/mongoose/mongoose.h
#        server/blink1-tiny-server.c
#        server/Dictionary.c
#        server/Dictionary.h
#        blink1-lib-lowlevel-hidapi.h
#        blink1-lib-lowlevel-hiddata.h
        blink1-lib.c
        blink1-lib.h
        blink1-tool.c
        hiddata.c
        hiddata.h)



### The uninstall target
if(NOT TARGET uninstall)
    configure_file(
            "${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
            "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
            IMMEDIATE @ONLY)

    add_custom_target(uninstall ${CMAKE_ROOT}
            COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
endif()
