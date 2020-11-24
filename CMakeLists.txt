cmake_minimum_required(VERSION 3.8)
project(hpp_skel LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED on)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(FetchContent)

SET(CMAKE_CXX_FLAGS "/Zi /sdl /Oi /Gy /permissive- /MP /GR /TP /EHsc")                     # TODO: /GR should be active only on editor mode
SET(CMAKE_CXX_FLAGS_DEBUG "/ZI /sdl /JMC /permissive- /MP /GR /TP /EHsc /MDd /Od /bigobj") # TODO: /GR should be active only on editor mode

# LIB DATA COMPRESSION LIBRARY
FetchContent_Declare(
  zlib
  GIT_REPOSITORY https://github.com/RodrigoHolztrattner/zlib
)

FetchContent_GetProperties(zlib)
if(NOT zlib_POPULATED)
  FetchContent_Populate(zlib)

     # Generate the sources.
     execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" -DCMAKE_INSTALL_PREFIX:PATH=${PACKAGES_ROOT}
     WORKING_DIRECTORY "${zlib_SOURCE_DIR}")

    # Build and install.
    execute_process(COMMAND ${CMAKE_COMMAND} --build . --target install WORKING_DIRECTORY "${zlib_SOURCE_DIR}") 
endif()

set(PROJECT_NAME "gzip")

project(PROJECT_NAME)

file (GLOB_RECURSE HEADER_FILES "include/gzip"/*.hpp "include/gzip"/*.h) 

add_library(${PROJECT_NAME} STATIC ${HEADER_FILES} "src/includes.cpp")

include_directories(${PROJECT_NAME} ${zlib_SOURCE_DIR})

include_directories("${PROJECT_SOURCE_DIR}/include")

target_link_libraries(${PROJECT_NAME} zlib)

# Request C++ 20
set_target_properties(${PROJECT_NAME} PROPERTIES
  CXX_STANDARD 20
  CXX_STANDARD_REQUIRED ON)