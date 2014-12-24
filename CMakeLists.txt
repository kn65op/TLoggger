cmake_minimum_required (VERSION 2.8.4) 

#variables
set(SRC_DIR "Code")
#set(MAIN_SRC_DIR "${SRC_DIR}/Main")
#set(MAIN_FILE "${MAIN_SRC_DIR}/main.cpp")
set(DEBUG_BIN_DIR "Debug")
set(RELEASE_BIN_DIR "Release")
set(LIB_NAME "TLogger")

#project
project (tlogger)

#externial libs
#FIND_PACKAGE( Boost 1.54 COMPONENTS date_time REQUIRED )
##FIND_PACKAGE( Boost 1.53 COMPONENTS program_options REQUIRED )

#compiler options
set(CMAKE_CXX_FLAGS "-std=c++1y")

if ("${NO_COVERAGE}" STREQUAL "true")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
else ("${NO_COVERAGE}" STREQUAL "true")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fprofile-arcs -ftest-coverage")
endif ("${NO_COVERAGE}" STREQUAL "true")

set(NORMAL_WARNING_FLAGS "${CMAKE_CXX_FLAGS_DEBUG}")
set(EXTRA_WARNING_FLAGS "${CMAKE_CXX_FLAGS_DEBUG} -Wall -Weffc++ -Wextra")

set(CMAKE_CXX_FLAGS_DEBUG "${EXTRA_WARNING_FLAGS}")

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -Wall")

#tests
set(COVERAGE_COMMAND "gcov")

set(CTEST_MEMORYCHECK_COMMAND "valgrind")
set(CTEST_MEMORYCHECK_COMMAND_OPTIONS "--trace-children=yes --quiet
--tool=memcheck --leak-check=yes --show-reachable=yes
--num-callers=100 --verbose --demangle=yes")

#includes
include_directories(${SRC_DIR})

#files
file(GLOB TLOGGER_LIB_FILES ${SRC_DIR}/TLogger/src/*.cpp)
#file(GLOB MAIN_FILES ONE_MINUTE_CHANGES_FILES ${SRC_DIR}/Main/src/*.cpp)

#executables
if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
	if ("${NO_COVERAGE}" STREQUAL "true")
	else ("${NO_COVERAGE}" STREQUAL "true")
		add_definitions(--coverage)
	endif ("${NO_COVERAGE}" STREQUAL "true")
	file(MAKE_DIRECTORY ${DEBUG_BIN_DIR})
	set(LIBRARY_OUTPUT_DIRECTORY ${DEBUG_BIN_DIR})
	set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${DEBUG_BIN_DIR})
elseif (${CMAKE_BUILD_TYPE} STREQUAL "Release")
	file(MAKE_DIRECTORY ${RELEASE_BIN_DIR})
	set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${RELEASE_BIN_DIR})
else (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
	message(FATAL_ERROR "Invalid build type. Can be Debug | Release")
endif (${CMAKE_BUILD_TYPE} STREQUAL "Debug")

#add_executable(${LIB_OUTPUT_FILE} ${MAIN_FILE} ${MAIN_FILES} ${ONE_MINUTE_CHANGES_FILES})
add_library(${LIB_NAME} ${TLOGGER_LIB_FILES})

#libraries
#TARGET_LINK_LIBRARIES(${LIB_OUTPUT_FILE} ${Boost_LIBRARIES} )
