cmake_minimum_required (VERSION 2.8.11)

project(OpenDTAM)
string(TOUPPER "${PROJECT_NAME}" PROJECT_NAME_UPPER)

#----------------------------BUILD CONFIGURATIONS------------------------------#
set(DEBUG_BUILD_TYPE_OPTIONS Debug RelWithDebInfo)
set(RELEASE_BUILD_TYPE_OPTIONS Release MinSizeRel)
set(BUILD_TYPE_OPTIONS ${DEBUG_BUILD_TYPE_OPTIONS} ${RELEASE_BUILD_TYPE_OPTIONS})

#set build type
set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build, options are: ${BUILD_TYPE_OPTIONS}.")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS ${BUILD_TYPE_OPTIONS})
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE})

#set build paths
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE})
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib/${CMAKE_BUILD_TYPE})
#---------------------------SUBDIRECTORIES-------------------------------------#
add_subdirectory(Cpp)


