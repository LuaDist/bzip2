# Copyright (C) 2007-2012 LuaDist.
# Submitted by David Manura
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project ( bzip2 C )
cmake_minimum_required ( VERSION 2.6 )
include ( cmake/dist.cmake )

add_definitions ( -D_FILE_OFFSET_BITS=64 )

# Library
set ( BZIP2_SRCS blocksort.c huffman.c crctable.c randtable.c compress.c decompress.c 
  bzlib.c )

add_library ( bz2 ${BZIP2_SRCS} libbz2.def )

add_executable ( bzip2 bzip2.c )
target_link_libraries ( bzip2 bz2 )

add_executable ( bzip2recover bzip2recover.c )

file ( WRITE bzegrep.1 ".so man1/bzgrep.1" )
file ( WRITE bzfgrep.1 ".so man1/bzgrep.1" )
file ( WRITE bzless.1 ".so man1/bzmore.1" )
file ( WRITE bzcmp.1 ".so man1/bzdiff.1" )

include ( CTest )
add_test ( test ${CMAKE_COMMAND} -P test.cmake )

install_executable ( bzip2 bzip2recover )
install_library ( bz2 )
install_header ( bzlib.h )
install_doc ( manual.html bzip2.1 bzgrep.1 bzmore.1 bzdiff.1 bzegrep.1 bzfgrep.1 
  bzless.1 bzcmp.1 )
install_data ( README LICENSE )
