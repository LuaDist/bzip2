# Copyright (C) 2007-2009 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(bzip2 C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE(dist.cmake)

ADD_DEFINITIONS(-D_FILE_OFFSET_BITS=64)

# Library
SET(BZIP2_SRCS blocksort.c huffman.c crctable.c randtable.c
               compress.c decompress.c bzlib.c )

ADD_LIBRARY(bz2 SHARED ${BZIP2_SRCS} libbz2.def)

ADD_EXECUTABLE(bzip2 bzip2.c)
TARGET_LINK_LIBRARIES(bzip2 bz2)

ADD_EXECUTABLE(bzip2recover bzip2recover.c)

FILE(WRITE bzegrep.1 ".so man1/bzgrep.1")
FILE(WRITE bzfgrep.1 ".so man1/bzgrep.1")
FILE(WRITE bzless.1 ".so man1/bzmore.1")
FILE(WRITE bzcmp.1 ".so man1/bzdiff.1")

INCLUDE(CTest)
ADD_TEST(test ${CMAKE_COMMAND} -P test.cmake)

INSTALL(TARGETS bzip2 bzip2recover bz2 RUNTIME DESTINATION ${INSTALL_BIN} LIBRARY DESTINATION ${INSTALL_LIB} ARCHIVE DESTINATION ${INSTALL_LIB})
INSTALL(FILES bzlib.h DESTINATION ${INSTALL_INC})
INSTALL(FILES README LICENSE manual.html bzip2.1 bzgrep.1 bzmore.1 bzdiff.1
              bzegrep.1 bzfgrep.1 bzless.1 bzcmp.1
              DESTINATION ${INSTALL_DOC})
INSTALL(PROGRAMS bzgrep bzmore bzdiff DESTINATION ${INSTALL_BIN}) #~2DO: windows versions?

#~2DO: improve with symbolic links
GET_TARGET_PROPERTY(BZIP2_LOCATION bzip2 LOCATION)
INSTALL(PROGRAMS ${BZIP2_LOCATION} DESTINATION ${INSTALL_BIN} RENAME bunzip2)
INSTALL(PROGRAMS ${BZIP2_LOCATION} DESTINATION ${INSTALL_BIN} RENAME bzcat)
INSTALL(PROGRAMS bzgrep DESTINATION ${INSTALL_BIN} RENAME bzegrep)
INSTALL(PROGRAMS bzgrep DESTINATION ${INSTALL_BIN} RENAME bzfgrep)
INSTALL(PROGRAMS bzmore DESTINATION ${INSTALL_BIN} RENAME bzless)
INSTALL(PROGRAMS bzdiff DESTINATION ${INSTALL_BIN} RENAME bzcmp)

#~2DO? build manual.ps and manual.pdf

