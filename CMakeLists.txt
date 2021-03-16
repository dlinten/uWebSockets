# Works with 3.11 and tested through 3.18
cmake_minimum_required(VERSION 3.14...3.18)

# Project name and a few useful settings. Other commands can pick up the results
project(uWebSockets
        VERSION 0.1
        DESCRIPTION "A C/C++ uWebSockets CMake Implementation"
        LANGUAGES CXX C)

if(${CMAKE_VERSION} VERSION_LESS 3.14)
    include(cmake/add_FetchContent_MakeAvailable.cmake)
endif()

include(FetchContent)

#############################################################
## C++ Code generator for model and service scaffolding
## Must use deploy token as this is built from within a
## container
#############################################################
#FetchContent_Declare(
#        usockets
#        GIT_REPOSITORY https://github.com/dlinten/uSockets.git
#        GIT_TAG        master
#)
#FetchContent_MakeAvailable(uwebsockets)

find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)

add_subdirectory(./uSockets)

add_library(uwebsocket INTERFACE)

# We need this directory, and users of our library will need it too
target_include_directories(uwebsocket INTERFACE ./src ${CMAKE_CURRENT_BINARY_DIR}/uSockets/include)

# This depends on (header only) boost
#target_link_libraries(uwebsocket PRIVATE Threads::Threads OpenSSL::SSL libusockets)

# All users of this library will need at least C++17
#target_compile_features(uwebsocket PUBLIC cxx_std_20)

#add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/uWebSockets.a
#        COMMAND env "MAKEFLAGS=$(subst s,,$(MAKEFLAGS)) WITH_OPENSSL=1" make -f ./Makefile
#        COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/uWebSockets.a  ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/uWebSockets.a
#        COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/include
#        COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/src/*.h ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/include/
#        COMMAND ${CMAKE_COMMAND} -E remove -f ${CMAKE_CURRENT_SOURCE_DIR}/*.a ${CMAKE_CURRENT_SOURCE_DIR}/*.o
#        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

#add_custom_target(uWebSockets ALL
#        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/uWebSockets.a)
#
#set_target_properties(uWebSockets
#        PROPERTIES
#        IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/uWebSockets.a
#        INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_BINARY_DIR}/uWebSockets/include)
