# CMake 最低版本号要求

cmake_minimum_required (VERSION 3.0)

# 项目信息
project (SimPlayer)

# 指定生成目标
MESSAGE(STATUS "operation system is ${CMAKE_SYSTEM}")  

 IF(CMAKE_CL_64)
    set(SIM_P x64)
ELSE(CMAKE_CL_64)
    set(SIM_P x86)
ENDIF(CMAKE_CL_64)

IF (CMAKE_SYSTEM_NAME MATCHES "Linux")  
    MESSAGE(STATUS "current platform: Linux ")  
	set(SIM_S linux)
	list(APPEND DEP_LIBS pthread)
ELSEIF (CMAKE_SYSTEM_NAME MATCHES "Windows")  
    MESSAGE(STATUS "current platform: Windows") 
	set(SIM_S windows)
ELSE ()  
    MESSAGE(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")  
ENDIF (CMAKE_SYSTEM_NAME MATCHES "Linux")  

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

#Sim
include_directories(${CMAKE_CURRENT_SOURCE_DIR}3rdparty/Sim/Sim/include)

#ffmpeg
include_directories(${CMAKE_CURRENT_SOURCE_DIR}3rdparty/ffmpeg/${SIM_S}/${SIM_P}/include)
link_directories(${CMAKE_CURRENT_SOURCE_DIR}3rdparty/ffmpeg/${SIM_S}/${SIM_P}/lib)

list(APPEND DEP_LIBS libavcodec.a libavdevice.a libavfilter.a libavformat.a libavutil.a libswresample.a libswscale.a)

MESSAGE(STATUS "DEP_LIBS: ${DEP_LIBS}") 

file(GLOB_RECURSE SRC_LIST ${CMAKE_CURRENT_SOURCE_DIR}/*.c ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp ${CMAKE_CURRENT_SOURCE_DIR}/*.h ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp)

add_library(${PROJECT_NAME} ${SRC_LIST})
target_link_libraries(${PROJECT_NAME}  ${DEP_LIBS})

#add_subdirectory(demo)
#option(BUILD_TESTS "Build tests." TRUE)
#if (BUILD_TESTS)
#    enable_testing()
#    add_subdirectory (test)
#endif()