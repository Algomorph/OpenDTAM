cmake_minimum_required (VERSION 2.8.11)
#set(CMAKE_BUILD_TYPE Debug) 


#set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O3 -g -ggdb")

find_package( OpenCV 2.4 REQUIRED )
find_package( Qt5Core REQUIRED )
find_package( CUDA REQUIRED )
find_package( Boost COMPONENTS system thread )

set(BASEPATH "${CMAKE_CURRENT_SOURCE_DIR}")

macro ( add_sources )
    file (RELATIVE_PATH _relPath "${BASEPATH}" "${CMAKE_CURRENT_SOURCE_DIR}")
    foreach (_src ${ARGN})
        if (_relPath)
            list (APPEND DTAM_SOURCES "${_relPath}/${_src}")
        else()
            list (APPEND DTAM_SOURCES "${_src}")
        endif()
    endforeach()
    if (_relPath)
        # propagate SRCS to parent directory
        set (DTAM_SOURCES ${DTAM_SOURCES} PARENT_SCOPE)
    endif()
endmacro( add_sources )

add_subdirectory( CostVolume )
add_subdirectory( Track )
add_subdirectory( Optimizer )
add_subdirectory( utils )
add_subdirectory( Scheduler )



include_directories("${BASEPATH}" ${CUDA_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

#---------------------CUDA FLAGS-----------------------------#
set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};-gencode arch=compute_35,code=sm_35;--use_fast_math ;--restrict; -lineinfo; -O0)

message(STATUS ${DTAM_SOURCES})

#----------------------DEFINE TARGETS------------------------#
cuda_add_library(${PROJECT_NAME} SHARED ${DTAM_SOURCES} OpenDTAM.cpp)

add_executable (a.out testprog.cpp convertAhandaPovRayToStandard.cpp graphics.cpp)
target_link_libraries( a.out  ${PROJECT_NAME} ${OpenCV_LIBS} ${Boost_LIBRARIES})

#----------------------INSTALLATION DETAILS------------------#
install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}Targets 
		RUNTIME DESTINATION bin
		LIBRARY DESTINATION lib
)
#TODO: add uninstall

#----------------------PACKAGE DETAILS-----------------------#
include(CPack)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "OpenDTAM, an open-source implementation of Dense Tracking and Mapping")

