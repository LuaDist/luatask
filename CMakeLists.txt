# Copyright (C) 2007-2009 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(luatask C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE(dist.cmake)

# Detect threading support.
# TODO: Windows can use Pthreads-Win32.
FIND_PACKAGE(Threads)

IF(CMAKE_USE_WIN32_THREADS_INIT)
	ADD_DEFINITIONS(-DNATV_WIN32 "-DLUATASK_API=__declspec(dllexport)")
ELSEIF(CMAKE_USE_PTHREADS_INIT)
	ADD_DEFINITIONS(-DLUATASK_API= -DLUATASK_PTHREAD_STACK_SIZE=2097152/16)
	SET(LIBS pthread)
ENDIF()

# Build modules
ADD_LUA_MODULE(task src/LuaTask/ltask.c src/LuaTask/queue.c src/LuaTask/syncos.c)
TARGET_LINK_LIBRARIES(task ${LIBS})

#~2DO: run tests?

# Install all files and documentation
INSTALL(TARGETS task DESTINATION ${INSTALL_CMOD})
INSTALL(FILES ChangeLog LICENSE README DESTINATION ${INSTALL_DATA})
INSTALL(DIRECTORY doc/ DESTINATION ${INSTALL_DOC})
INSTALL(DIRECTORY examples/ DESTINATION ${INSTALL_EXAMPLE})
