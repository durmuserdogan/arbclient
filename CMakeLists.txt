cmake_minimum_required(VERSION 3.7)


project(arbclient)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${arbclient_SOURCE_DIR}/cmake")
set(BIN_DIR ${arbclient_SOURCE_DIR}/)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/)

# Bump up warning levels appropriately for clang, gcc & msvc and build in debug mode
if (MSVC)
	if (CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
		string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
	else()
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
	endif()
else()
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -pedantic -std=c++11")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_DEBUG} -g")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_RELEASE} -O2")
endif()

find_package(MOSQUITTO REQUIRED)
include_directories(${MOSQUITTO_INCLUDE_DIR})



add_executable(arbclient src/main.cpp )


target_link_libraries(arbclient ${MOSQUITTO_LIBRARY} )
install(TARGETS arbclient RUNTIME DESTINATION ${BIN_DIR})