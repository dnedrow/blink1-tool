cmake_minimum_required(VERSION 3.16)
project(blink1_tool C)

set(CMAKE_C_STANDARD 11)

include_directories(.)
include_directories(blink1-mini-tool)
include_directories(server)
include_directories(server/mongoose)

add_executable(blink1_tool
        blink1-mini-tool/blink1-mini-tool.c
        blink1-mini-tool/hiddata.c
        blink1-mini-tool/hiddata.h
        blink1control-tool/blink1control-tool.c
        blink1raw/blink1raw.c
        server/mongoose/mongoose.c
        server/mongoose/mongoose.h
        server/blink1-tiny-server.c
        server/Dictionary.c
        server/Dictionary.h
        blink1-lib-lowlevel-hidapi.h
        blink1-lib-lowlevel-hiddata.h
        blink1-lib.c
        blink1-lib.h
        blink1-tool.c
        hiddata.c
        hiddata.h)
