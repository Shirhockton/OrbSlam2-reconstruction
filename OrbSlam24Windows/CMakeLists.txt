cmake_minimum_required(VERSION 2.8)
project(ORB_SLAM2)


SET(CMAKE_BUILD_TYPE Release)
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -W0 -MT -bigobj")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -W0 -MT -bigobj")

# Check C++11 or C++0x support
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)

set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

#SET(DBUILD_SHARED false)

if(COMPILER_SUPPORTS_CXX11)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   add_definitions(-DCOMPILEDWITHC11)
   message(STATUS "Using flag -std=c++11.")
elseif(COMPILER_SUPPORTS_CXX0X)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
   add_definitions(-DCOMPILEDWITHC0X)
   message(STATUS "Using flag -std=c++0x.")
#else()
#   message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()


LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)


find_package(Pangolin REQUIRED)

include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${PROJECT_SOURCE_DIR}/Thirdparty/eigen3.2.9




#使用Pangolin
${Pangolin_INCLUDE_DIRS}
${PROJECT_SOURCE_DIR}/Thirdparty/Pangolin/include
${PROJECT_SOURCE_DIR}/Thirdparty/Pangolin/build/src
${PROJECT_SOURCE_DIR}/Thirdparty/Pangolin/build/external/glew/include

${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/include
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/thirdParty
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/thirdparty/miniBoost
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/thirdParty/graclus1.2/metisLib
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/thirdParty/nlopt-2.4.2/api
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/thirdParty/cimg
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/thirdParty/tinycthread

${PROJECT_SOURCE_DIR}/Thirdparty/ReconstuctMesh/include

${PROJECT_SOURCE_DIR}/Thirdparty/opencv3.1.0/build/include
${PROJECT_SOURCE_DIR}/Thirdparty/opencv3.1.0/build/include/opencv
${PROJECT_SOURCE_DIR}/Thirdparty/opencv3.1.0/build/include/opencv2
)


set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_library(${PROJECT_NAME} STATIC
src/System.cc
src/Tracking.cc
src/LocalMapping.cc
src/LoopClosing.cc
src/ORBextractor.cc
src/ORBmatcher.cc
src/FrameDrawer.cc
src/Converter.cc
src/MapPoint.cc
src/KeyFrame.cc
src/Map.cc
src/MapDrawer.cc
src/Optimizer.cc
src/PnPsolver.cc
src/Frame.cc
src/KeyFrameDatabase.cc
src/Sim3Solver.cc
src/Initializer.cc
src/Viewer.cc
src/Factory.cpp
)



LINK_DIRECTORIES(
${PROJECT_SOURCE_DIR}/lib
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib
${PROJECT_SOURCE_DIR}/Thirdparty/Pangolin/lib
${PROJECT_SOURCE_DIR}/Thirdparty/CXSparse/lib
${PROJECT_SOURCE_DIR}/Thirdparty/PointCloud/lib


# 构网模块
${PROJECT_SOURCE_DIR}/Thirdparty/ReconstuctMesh/lib
${PROJECT_SOURCE_DIR}/Thirdparty/ReconstuctMesh/Thirdparty/cgal-releases-CGAL-4.8.1/build/lib

${PROJECT_SOURCE_DIR}/Thirdparty/opencv3.1.0/build/x64/vc14/lib

)

target_link_libraries(${PROJECT_NAME}
${Pangolin_LIBRARIES}
pangolin.lib
DBoW2
g2o
image_lib
jpeg
nlopt
numeric_lib
pmvs_lib
PointCloud
tinycthread


libCGAL-vc140-mt-4.8.1-I-900
${PROJECT_SOURCE_DIR}/Thirdparty/ReconstuctMesh/Thirdparty/libgmp-10.lib
ReconstuctMesh.lib
opencv_world310
)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Examples/Monocular)
#set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Examples/Stereo)

add_executable(mono_tum
Examples/Monocular/mono_tum.cc)
target_link_libraries(mono_tum ${PROJECT_NAME})

add_executable(mono_camera
Examples/Monocular/mono_camera.cc)
target_link_libraries(mono_camera ${PROJECT_NAME})


#add_executable(stereo_euroc Examples/Stereo/stereo_euroc.cc
#Examples/Stereo/ViewerAR.cc
#Examples/Stereo/ViewerAR.h
#)
#target_link_libraries(stereo_euroc ${PROJECT_NAME})

