cmake_minimum_required (VERSION 3.5.1)
project (demo)
set(CMAKE_C_FLAGS "${CMAKE_CFLAGS} -Wall -O3 -mcmodel=medium")
include_directories (../common)
LINK_LIBRARIES(m)
#aux_source_directory (libcheckpoint SRC_LIST)
aux_source_directory (../../libcheckpoint CPK_LIST)
aux_source_directory (../common COM_LIST)
aux_source_directory (src SRC_LIST)
set(cpk_directory "build/cpk_directory")
#set(clean ./
file(MAKE_DIRECTORY ${cpk_directory})
file(COPY ../delete.sh DESTINATION ./)
#add_custom_target(mytargetname ALL COMMAND ${CMAKE_COMMAND} -E make_directory ${cpk_directory})
SET(CMAKE_C_COMPILER "/usr/bin/gcc")
SET(CMAKE_CXX_COMPILER "/usr/bin/g++")
add_executable (run  ${SRC_LIST} ${COM_LIST} ${CPK_LIST} )
