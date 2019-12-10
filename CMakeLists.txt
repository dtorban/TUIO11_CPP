# This file is part of the MinVR cmake build system.  
# See the main MinVR/CMakeLists.txt file for authors, copyright, and license info.
#
# Code author(s):
# 		Dan Orban (dtorban)
#

cmake_minimum_required (VERSION 2.8)
project (TUIO)

if(NOT CMAKE_DEBUG_POSTFIX)
  set(CMAKE_DEBUG_POSTFIX "d")
endif()
set(CMAKE_RELEASE_POSTFIX "")
set(CMAKE_RELWITHDEBINFO_POSTFIX "rd")
set(CMAKE_MINSIZEREL_POSTFIX "s")

if (CMAKE_BUILD_TYPE MATCHES "Release")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_RELEASE_POSTFIX}")
elseif (CMAKE_BUILD_TYPE MATCHES "MinSizeRel")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_MINSIZEREL_POSTFIX}")
elseif (CMAKE_BUILD_TYPE MATCHES "RelWithDebInfo")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_RELWITHDEBINFO_POSTFIX}")
elseif (CMAKE_BUILD_TYPE MATCHES "Debug")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_DEBUG_POSTFIX}")
else()
  set(CMAKE_BUILD_POSTFIX "")
endif()

set(SOURCEFILES 
  TUIO/FlashSender.cpp
  TUIO/FlashSender.h
  TUIO/LibExport.h
  TUIO/OneEuroFilter.cpp
  TUIO/OneEuroFilter.h
  TUIO/OscReceiver.cpp
  TUIO/OscReceiver.h
  TUIO/OscSender.h
  TUIO/TcpReceiver.cpp
  TUIO/TcpReceiver.h
  TUIO/TcpSender.cpp
  TUIO/TcpSender.h
  TUIO/TuioBlob.cpp
  TUIO/TuioBlob.h
  TUIO/TuioClient.cpp
  TUIO/TuioClient.h
  TUIO/TuioContainer.cpp
  TUIO/TuioContainer.h
  TUIO/TuioCursor.cpp
  TUIO/TuioCursor.h
  TUIO/TuioDispatcher.cpp
  TUIO/TuioDispatcher.h
  TUIO/TuioListener.h
  TUIO/TuioManager.cpp
  TUIO/TuioManager.h
  TUIO/TuioObject.cpp
  TUIO/TuioObject.h
  TUIO/TuioPoint.cpp
  TUIO/TuioPoint.h
  TUIO/TuioServer.cpp
  TUIO/TuioServer.h
  TUIO/TuioTime.cpp
  TUIO/TuioTime.h
  TUIO/UdpReceiver.cpp
  TUIO/UdpReceiver.h
  TUIO/UdpSender.cpp
  TUIO/UdpSender.h
  TUIO/WebSockSender.cpp
  TUIO/WebSockSender.h


  oscpack/ip/IpEndpointName.cpp
  oscpack/ip/IpEndpointName.h
  oscpack/ip/NetworkingUtils.h
  oscpack/ip/PacketListener.h
  oscpack/ip/TimerListener.h
  oscpack/ip/UdpSocket.h

  oscpack/osc/MessageMappingOscPacketListener.h
  oscpack/osc/OscException.h
  oscpack/osc/OscHostEndianness.h
  oscpack/osc/OscOutboundPacketStream.cpp
  oscpack/osc/OscOutboundPacketStream.h
  oscpack/osc/OscPacketListener.h
  oscpack/osc/OscPrintReceivedElements.cpp
  oscpack/osc/OscPrintReceivedElements.h
  oscpack/osc/OscReceivedElements.cpp
  oscpack/osc/OscReceivedElements.h
  oscpack/osc/OscTypes.cpp
  oscpack/osc/OscTypes.h
)

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  set(SOURCEFILES ${SOURCEFILES}
    oscpack/ip/win32/NetworkingUtils.cpp
    oscpack/ip/win32/UdpSocket.cpp
  )
else()
  set(SOURCEFILES ${SOURCEFILES}
    oscpack/ip/posix/NetworkingUtils.cpp
    oscpack/ip/posix/UdpSocket.cpp
  )
endif()  

set (CMAKE_VERBOSE_MAKEFILE TRUE)

# Used for compiling in subdirectories of the project
include_directories (
  .
  oscpack
)


############################################################
#OS Specific flags should go here
############################################################

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++11")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
  #set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 -stdlib=libc++")
endif()

if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  # Linux specific code
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -std=c++11") 
  add_definitions("-DOSC_HOST_LITTLE_ENDIAN")
endif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  # Windows specific code
endif (${CMAKE_SYSTEM_NAME} MATCHES "Windows")

############################################################
#Finally add all the targets here
############################################################

add_library(TUIO ${SOURCEFILES})

#------------------------------------------
# Install Target
#------------------------------------------

install( TARGETS ${PROJECT_NAME}
         LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
         ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
         RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/TUIO/ DESTINATION "${CMAKE_INSTALL_PREFIX}/include/TUIO")
install(DIRECTORY ${PROJECT_SOURCE_DIR}/oscpack/ip DESTINATION "${CMAKE_INSTALL_PREFIX}/include/TUIO")
install(DIRECTORY ${PROJECT_SOURCE_DIR}/oscpack/osc DESTINATION "${CMAKE_INSTALL_PREFIX}/include/TUIO")

