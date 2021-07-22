cmake_minimum_required(VERSION 3.10 FATAL_ERROR)

project(rtspLive555)

set(CMAKE_BUILD_TYPE Debug)
#set(CMAKE_BUILD_TYPE Release)
#set(CMAKE_VERBOSE_MAKEFILE ON)
#set(-DCMAKE_TOOLCHAIN_FILE=toolchain-hisiv400.cmake)

#set(CMAKE_CXX_STANDARD 11)

add_definitions(
	-Wall
	-DBSD=1
	#-DDEBUG
	-DSOCKLEN_T=socklen_t
	-D_LARGEFILE_SOURCE=1
	-D_FILE_OFFSET_BITS=64
	-DALLOW_RTSP_SERVER_PORT_REUSE=1
)

include_directories(
	groupsock/include
	liveMedia/include
	UsageEnvironment/include
	BasicUsageEnvironment/include
)

link_libraries(-lssl -lcrypto)

file(GLOB_RECURSE SOURCE_FILE
	groupsock/*.c
	groupsock/*.cpp
	liveMedia/*.c
	liveMedia/*.cpp
	UsageEnvironment/*.cpp
	BasicUsageEnvironment/*.cpp
	testProgs/announceURL.cpp
)

add_library(objlib OBJECT ${SOURCE_FILE})
link_libraries(objlib)

add_executable(testMKVStreamer testProgs/testMKVStreamer.cpp)
add_executable(testMKVSplitter testProgs/testMKVSplitter.cpp)
add_executable(testOnDemandRTSPServer testProgs/testOnDemandRTSPServer.cpp)
