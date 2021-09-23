cmake_minimum_required(VERSION 2.8)

project(live555)
find_package(OpenSSL REQUIRED)
include_directories(${CMAKE_SOURCE_DIR}/BasicUsageEnvironment/include)
include_directories(${CMAKE_SOURCE_DIR}/groupsock/include)
include_directories(${CMAKE_SOURCE_DIR}/liveMedia/include)
include_directories(${CMAKE_SOURCE_DIR}/UsageEnvironment/include)

if(MSVC)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_WINSOCK_DEPRECATED_NO_WARNINGS)
add_definitions(-DNO_GETIFADDRS)
endif()

file(GLOB BasicUsageEnvironmentFiles
	BasicUsageEnvironment/*.cpp)
add_library(BasicUsageEnvironment
			${BasicUsageEnvironmentFiles})
			
file(GLOB groupsockfiles
	groupsock/*.cpp groupsock/*.c)
add_library(groupsock ${groupsockfiles})

file(GLOB liveMediaFiles
		liveMedia/*.hh liveMedia/*.cpp liveMedia/*.c)	
add_library(liveMedia ${liveMediaFiles})
target_link_libraries(liveMedia OpenSSL::SSL)

file(GLOB UsageEnvironmentFiles 
    UsageEnvironment/*.cpp)
add_library(UsageEnvironment ${UsageEnvironmentFiles})

file(GLOB mediaServerFiles
	mediaServer/*.hh mediaServer/*.cpp)
add_executable(mediaServer ${mediaServerFiles})
if(MSVC)
	target_link_libraries(mediaServer ws2_32.lib)
endif()
target_link_libraries(mediaServer  
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)

add_executable(proxyServer proxyServer/live555ProxyServer.cpp)
if(MSVC)
	target_link_libraries(proxyServer ws2_32.lib)
endif()
target_link_libraries(proxyServer  
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)

add_executable(hlsProxy hlsProxy/live555HLSProxy.cpp)
if(MSVC)
	target_link_libraries(hlsProxy ws2_32.lib)
endif()
target_link_libraries(hlsProxy  
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)


add_executable(openRTSP testProgs/playCommon.cpp testProgs/openRTSP.cpp)
if(MSVC)
	target_link_libraries(openRTSP ws2_32.lib)
endif()
target_link_libraries(openRTSP  
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)

add_executable(playSIP testProgs/playCommon.cpp testProgs/playSIP.cpp)
if(MSVC)
	target_link_libraries(playSIP ws2_32.lib)
endif()
target_link_libraries(playSIP  
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)

add_executable(RTSPClient testProgs/testRTSPClient.cpp)
if(MSVC)
	target_link_libraries(RTSPClient ws2_32.lib)
endif()
target_link_libraries(RTSPClient  
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)


add_executable(testH265VideoStreamer testProgs/testH265VideoStreamer.cpp testProgs/announceURL.cpp)
if(MSVC)
	target_link_libraries(testH265VideoStreamer ws2_32.lib)
endif()
target_link_libraries(testH265VideoStreamer 
			BasicUsageEnvironment groupsock
			UsageEnvironment liveMedia)