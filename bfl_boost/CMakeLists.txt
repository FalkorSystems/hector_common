cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
set(ROS_BUILD_TYPE Release)

rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

option(USE_SVN "Use SVN version of BFL instead of the tarball." OFF)
if (USE_SVN)
  execute_process(COMMAND cmake -E chdir ${PROJECT_SOURCE_DIR} make -f Makefile.bfl.svn
                  RESULT_VARIABLE _make_failed)
  if(_make_failed)
    message(FATAL_ERROR "Checkout of BFL failed")
  endif(_make_failed)
else (USE_SVN)
  execute_process(COMMAND cmake -E chdir ${PROJECT_SOURCE_DIR} make -f Makefile.bfl.tarball
                  RESULT_VARIABLE _make_failed)
  if(_make_failed)
    message(FATAL_ERROR "Download of BFL failed")
  endif(_make_failed)
endif (USE_SVN)

set(CMAKE_INSTALL_PREFIX "${PROJECT_SOURCE_DIR}/install")
set(BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(MATRIX_LIB boost)
set(RNG_LIB boost)

set(SOURCE_DIR "orocos-bfl-0.8.0")
add_subdirectory(build/${SOURCE_DIR} ${SOURCE_DIR})

