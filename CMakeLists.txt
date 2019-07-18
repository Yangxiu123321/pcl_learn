cmake_minimum_required(VERSION 3.0)

# Project Name
set(PROJ_NAME pcl_learn)
project(${PROJ_NAME})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

FILE(GLOB HEADER_LIST "${CMAKE_SOURCE_DIR}/src/*.h")
FILE(GLOB SOURCE_LIST "${CMAKE_SOURCE_DIR}/src/*.cpp")

add_executable(${PROJ_NAME} ${SOURCE_LIST} ${HEADER_LIST})
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJ_NAME})

# OpenCV
#set (OpenCV_DIR "C:/ST42Data/Code/opencv3Src/install/x86/vc15/lib") 
# 更改此路径为OpenCVConfig.cmake的路径（库目录）
find_package(OpenCV REQUIRED)
if(OpenCV_FOUND)
  include_directories(${OpenCV_INCLUDE_DIRS})
  target_link_libraries(${PROJ_NAME} ${OpenCV_LIBS})
endif()

# PCL
find_package(PCL 1.9.1 REQUIRED)
if(PCL_FOUND)
    include_directories(${PCL_INCLUDE_DIRS})
    add_definitions(${PCL_DEFINITIONS})
    link_directories(${PCL_LIBRARY_DIRS})
    target_link_libraries(${PROJ_NAME} ${PCL_LIBRARIES})
endif()

# realSense
set(RealSense_DIR "C:/Program Files (x86)/Intel RealSense SDK 2.0")
set(RealSense_INCLUDE_DI "${RealSense_DIR}/include/librealsense2")
set(RealSense_INCLUDE_DIR "${RealSense_DIR}/include")
set(RealSense_LIB "${RealSense_DIR}/lib/x64/realsense2.lib")
include_directories(${PROJ_NAME} ${RealSense_INCLUDE_DIR} ${RealSense_INCLUDE_DI})
target_link_libraries(${PROJ_NAME} ${RealSense_LIB})

# 识别为c++代码
set_target_properties(${PROJ_NAME} PROPERTIES LINKER_LANGUAGE CXX)

# 多核处理
set_target_properties(${PROJ_NAME} PROPERTIES COMPILE_FLAGS "/MP")

# Administrator
#set_target_properties(${PROJ_NAME} PROPERTIES LINK_FLAGS "/level='requireAdministrator'") 

# Unicode编码
Add_Definitions(-DUNICODE -D_UNICODE)