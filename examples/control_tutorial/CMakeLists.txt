# 
#  Copyright (C) 2014 Walkman
#  Author: 
#  email: 
#
#  This program is free software: you can redistribute it and/or modify
#  it under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation, either version 3 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
#  GNU Lesser General Public License for more details.
#
#  You should have received a copy of the GNU Lesser General Public License
#  along with this program. If not, see <http://www.gnu.org/licenses/>
#

cmake_minimum_required(VERSION 2.8.12)
if(COMMAND cmake_policy)
    cmake_policy(SET CMP0003 OLD)
    cmake_policy(SET CMP0005 NEW)
    cmake_policy(SET CMP0017 NEW)
endif(COMMAND cmake_policy)
include(ExternalProject)
project(control_tutorial)

include(CheckCXXCompilerFlag)
check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
check_cxx_compiler_flag("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}")

FIND_PACKAGE(YARP REQUIRED)
FIND_PACKAGE(idynutils REQUIRED)
FIND_PACKAGE(paramHelp REQUIRED)
find_package(iDynTree REQUIRED)

# add include directories
INCLUDE_DIRECTORIES(include 
		    ${YARP_INCLUDE_DIRS} 
		    ${idynutils_INCLUDE_DIRS}
		    )
		    
ADD_EXECUTABLE(${PROJECT_NAME} src/control_tutorial_main.cpp 
			       src/control_tutorial_thread.cpp
			       )

TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${iDynTree_LIBRARIES}
                                      ${YARP_LIBRARIES}
				      ${idynutils_LIBRARIES}
				      ${paramHelp_LIBRARIES}
				      ${iDynTree_LIBRARIES}
				      GYM
				      )

add_subdirectory(app)

install(TARGETS ${PROJECT_NAME} DESTINATION bin)
