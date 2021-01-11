cmake_minimum_required(VERSION 2.8)
project(ydlidar)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
#include_directories(include)
#include_directories(src)
SET(CMAKE_SYSTEM_NAME Linux)
SET(CMAKE_C_COMPILER /root/rk3308_cross_toolchain/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc)
SET(CMAKE_CXX_COMPILER /root/rk3308_cross_toolchain/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-g++)

include_directories(include)
include_directories(src)
IF (WIN32)
FILE(GLOB SDK_SRC 
  "src/*.cpp"
  "src/*.c"
  "src/*.h"
  "src/impl/windows/*.cpp"
  "src/impl/windows/*.h"
)
	
ELSE()
FILE(GLOB SDK_SRC 
  "src/*.cpp"
  "src/*.c"
  "src/*.h"
  "src/impl/unix/*.cpp"
  "src/impl/unix/*.h"
)

	
ENDIF()

set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -s")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -s")


add_subdirectory(samples)

add_library(ydlidar_driver STATIC ${SDK_SRC})
IF (WIN32)
target_link_libraries(ydlidar_driver setupapi)
ELSE()
target_link_libraries(ydlidar_driver rt pthread)
ENDIF()                    
