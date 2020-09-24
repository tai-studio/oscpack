# Note that headers are optional, and do not affect add_library, but they will not
# show up in IDEs unless they are listed in add_library.

# Optionally glob, but only for CMake 3.12 or later:
file(GLOB HEADER_LIST CONFIGURE_DEPENDS "${Oscpack_SOURCE_DIR}/include/*/*.h")
# set(HEADER_LIST "${Oscpack_SOURCE_DIR}/include/modern/lib.hpp")

# separate versions of NetworkingUtils.cpp and UdpSocket.cpp are provided for Win32 and POSIX
# the IpSystemTypePath selects the correct ones based on the current platform
IF(WIN32)
 set(IpSystemTypePath ip/win32)
 set(LIBS ${LIBS} Ws2_32 winmm)
ELSE(WIN32)
 set(IpSystemTypePath ip/posix)
ENDIF(WIN32)

# Make an automatic library - will be static or dynamic based on user setting
add_library(oscpack 
	ip/IpEndpointName.cpp
	${IpSystemTypePath}/NetworkingUtils.cpp
	${IpSystemTypePath}/UdpSocket.cpp
	osc/OscTypes.cpp 
	osc/OscReceivedElements.cpp
	osc/OscPrintReceivedElements.cpp
	osc/OscOutboundPacketStream.cpp
)

# We need this directory, and users of our library will need it too
target_include_directories(oscpack PUBLIC ../include)

# This depends on (header only) boost
# target_link_libraries(oscpack PRIVATE Boost::boost)


if(MSVC)
  # Force to always compile with W4
  if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
    string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
  endif()
elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
  # Update if necessary
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wno-long-long -pedantic")
endif()

# All users of this library will need at least C++11
# target_compile_features(oscpack PUBLIC cxx_std_11)

# IDEs should put the headers in a nice place
source_group(
  TREE "${PROJECT_SOURCE_DIR}/include"
  PREFIX "Header Files"
  FILES ${HEADER_LIST}
)
