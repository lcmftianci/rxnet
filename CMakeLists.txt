#cmake versions
cmake_minimum_required(VERSION 2.8)
 
PROJECT (rxnet)

set (sfup_VERSION_MAJOR 0)
set (sfup_VERSION_MINOR 1)
set (sfup_VERSION_PATCH 1)

option(BUILD_SIGNAL_SERVER "Build signal server." OFF)
option(BUILD_MEDIA_SERVER "Build media server." ON)
option(BUILD_CONTROLLER_SERVER "Build controller server." ON)
 
SET(CMAKE_BUILD_TYPE "Debug")
 
SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS_DEBUG} -Wl,--rpath=./lib/ -m64 -Wall -o0 -g -ggdb -pthread -std=c++11 -fPIC")
 
#SET(SRC_LIST sfu_media/src/main.cpp)
aux_source_directory(${PROJECT_SOURCE_DIR}/samples/ FILEUTIL_SAMPLE)
aux_source_directory(${PROJECT_SOURCE_DIR}/src/core/serialize/ SERIALIZE)
aux_source_directory(${PROJECT_SOURCE_DIR}/src/core/fileutil/ FILEUTIL)

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})
 
#SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/out)
 
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/src/core/serialize/
		    ${PROJECT_SOURCE_DIR}/src/core/fileutil/ 
		    )
 
LINK_DIRECTORIES(${PROJECT_SOURCE_DIR}/lib  /usr/local/lib)
 
add_definitions(-D_TTS_)
add_definitions(-g)
 
#ADD_SUBDIRECTORY(sfu_core)
#ADD_SUBDIRECTORY(third_party/log4z)
#ADD_SUBDIRECTORY(third_party/websocketpp)
#ADD_SUBDIRECTORY(third_party/jsoncpp)
#ADD_SUBDIRECTORY(third_party/libuv)


#add_custom_target(TARGET ALL DEPENDS COPY_RES_MEDIA)

ADD_LIBRARY(fileutil SHARED ${FILEUTIL}) 
ADD_LIBRARY(serialize SHARED ${SERIALIZE}) 
ADD_EXECUTABLE(fileutil_sample ${FILEUTIL_SAMPLE})

TARGET_LINK_LIBRARIES(fileutil_sample fileutil)

#if(BUILD_SIGNAL_SERVER)
#	ADD_EXECUTABLE(sfu_signal_server ${SIGNAL_SRC_LIST})
#else()
#	message(WARNING "you select do not build signal server!")
#endif()

#add_custom_command(TARGET  fileutil_sample
#		POST_BUILD
#		COMMAND cp ${PROJECT_SOURCE_DIR}/sfu_media/sfu_media.json ${PROJECT_SOURCE_DIR}/out/)

#execute_process(COMMAND cd ${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/
#		COMMAND sh bootstrap.sh --prefix=${PROJECT_SOURCE_DIR}/out/boost/
#		)

#add_custom_command(TARGET  sfu_signal_server
#		PRE_BUILD
#		COMMAND cd ${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/
#		COMMAND sh bootstrap.sh --prefix=${PROJECT_SOURCE_DIR}/out/boost/
#		COMMAND ./b2 install --with-system --with-date_time --with-random --with-regex link=static runtime-link=shared threading=multi debug
#		COMMAND cd -)

#add_custom_command(TARGET  sfu_signal_server
#		PRE_BUILD
#		COMMAND sh ${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/bootstrap.sh --prefix=${PROJECT_SOURCE_DIR}/out/boost/
#		COMMAND mv ${PROJECT_BINARY_DIR}/b2 ${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/
#		COMMAND mv ${PROJECT_BINARY_DIR}/bjam ${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/
#		COMMAND mv ${PROJECT_BINARY_DIR}/project-config.jam ${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/
#		COMMAND echo "`${PROJECT_SOURCE_DIR}/third_party/boost_1_68_0/b2 install --with-system --with-date_time --with-random --with-regex link=static runtime-link=shared threading=multi debug`")

#if(BUILD_SIGNAL_SERVER)
#add_custom_command(TARGET  sfu_signal_server
#		POST_BUILD
#		COMMAND echo "copy config file"
#		COMMAND cp ${PROJECT_SOURCE_DIR}/sfu_signal_server/sfu_signal.json ${PROJECT_SOURCE_DIR}/out/)
#endif()
		
#TARGET_LINK_LIBRARIES(sfu_media sfu_core log4z)
#TARGET_LINK_LIBRARIES(sfu_media sfu_core log4z boost_system ssl crypto)

#ADD_EXECUTABLE(sfu_controller ${CONTROL_SRC_LIST})
#TARGET_LINK_LIBRARIES(sfu_controller uv)

#if(BUILD_SIGNAL_SERVER)
#TARGET_LINK_LIBRARIES(sfu_signal_server sfu_core log4z boost_date_time boost_system boost_filesystem boost_thread boost_random boost_wserialization ssl crypto jsoncpp_lib)
#TARGET_LINK_LIBRARIES(sfu_signal_server sfu_core log4z boost_system ssl crypto jsoncpp_lib)
#endif()
