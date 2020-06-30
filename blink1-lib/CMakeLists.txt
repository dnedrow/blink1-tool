cmake_minimum_required(VERSION 3.16)

set(CMAKE_C_STANDARD 11)

find_program(MAKE make)
include(FetchContent)
include(ExternalProject)

ExternalProject_Add(hidapi
        GIT_REPOSITORY  https://github.com/libusb/hidapi.git
        GIT_TAG         hidapi-0.9.0
        GIT_SHALLOW     TRUE
        GIT_PROGRESS    TRUE
        GIT_REMOTE_UPDATE_STRATEGY  CHECKOUT
        CONFIGURE_COMMAND ""
        INSTALL_COMMAND ""
        LOG_BUILD       TRUE
        EXCLUDE_FROM_ALL TRUE
        COMMAND       ${CMAKE_COMMAND} -E echo ${SOURCE_DIR}
        COMMAND       cd mac; ${MAKE} -d --makefile=Makefile-manual
        COMMAND       ${CMAKE_COMMAND} -E echo "$<CONFIG> build complete"
        )
ExternalProject_Get_Property(hidapi SOURCE_DIR)
#set(HIDAPI_SOURCE_DIR SOURCE_DIR)
message(STATUS "Hey2: ${SOURCE_DIR}")

#FetchContent_Declare(
#        hidapi
#        GIT_REPOSITORY  https://github.com/libusb/hidapi.git
#        GIT_TAG         hidapi-0.9.0
#        GIT_SHALLOW     TRUE
#        GIT_PROGRESS    TRUE
#        GIT_REMOTE_UPDATE_STRATEGY  CHECKOUT
#        BUILD_COMMAND ${CMAKE_COMMAND} -E echo "Starting $<CONFIG> build"
#        COMMAND       ${CMAKE_COMMAND} -E echo <SOURCE_DIR>
#        COMMAND       cd mac; ${MAKE} -d --makefile=Makefile-manual
#        COMMAND       ${CMAKE_COMMAND} -E echo "$<CONFIG> build complete"
#)
#FetchContent_MakeAvailable(hidapi)
#FetchContent_GetProperties(hidapi)
#message(STATUS "HIDAPI Source Dir: ${hidapi_SOURCE_DIR}")

# The first external project will be built at *configure stage*
#execute_process(
#        COMMAND "cd ${hidapi_SOURCE_DIR}/mac; ${MAKE} -d --makefile=Makefile-manual"
#)
#
project(Blink1 C)

#ExternalProject_Add(hidapi
#        GIT_REPOSITORY "https://github.com/libusb/hidapi.git"
##        UPDATE_COMMAND git pull "https://github.com/libusb/hidapi.git"
#        SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/hidapi
#        INSTALL_DIR ${CMAKE_CURRENT_BINARY_DIR}
#)

include_directories(.)
include_directories(hidapi/hidapi)

add_library(Blink1 SHARED src/blink1-lib.c)

target_include_directories(Blink1
        PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        )

#find_library(HIDAPI_LIB
#        NAMES hid hid.o
#        PATHS  ${SOURCE_DIR})
