cmake_minimum_required(VERSION 3.10)

project(bdmfs_fatfs LANGUAGES C)

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/source/include ${PS2SDK}/iop/include)

SET(IOP_BIN bdmfs_fatfs.irx)
SET(IOP_SRCS source/diskio.c source/ff.c source/ffsystem.c source/ffunicode.c source/fs_driver.c source/main.c imports.c)

BUILD_IOP_IMPORTS(imports.c ${CMAKE_CURRENT_SOURCE_DIR}/imports.lst)

SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -lbdm -lgcc -Wall -fno-builtin")
SET(CMAKE_LDFLAGS "${IOP_LDFLAGS} -lgcc -lbdm")
SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -s")

ADD_EXECUTABLE(${IOP_BIN} ${IOP_SRCS})

ADD_CUSTOM_COMMAND(TARGET ${IOP_BIN} POST_BUILD COMMAND md5sum ARGS ${IOP_BIN})

target_link_libraries(bdmfs_fatfs.irx
    PRIVATE
    gcc
    bdm
    )