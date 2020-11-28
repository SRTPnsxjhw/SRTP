/home/ji/code/rnng

g++ -o test test.cc -I /home/ji/code/boost_1_61_0 -L /home/ji/code/boost_1_61_0/stage/lib -no-pie

SET (BOOST_ROOT "/home/ji/code/boost_1_61_0") 
SET (Boost_INCLUDE_DIR "/home/ji/code/boost_1_61_0") 
SET (Boost_LIBRARIES "/home/ji/code/boost_1_61_0/stage/lib") 
SET (EIGEN3_INCLUDE_DIR "/home/ji/code/eigen")

```
project(cnn)
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(CMAKE_CXX_FLAGS "-Wall -std=c++11 -O3 -g")
SET (EIGEN3_INCLUDE_DIR "/home/ji/code/eigen")
enable_testing()

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/cnn)
set(WITH_EIGEN_BACKEND 1)

# look for Boost
set(Boost_REALPATH ON)
find_package(Boost COMPONENTS program_options iostreams serialization REQUIRED)
include_directories(${Boost_INCLUDE_DIR})
set(LIBS ${LIBS} ${Boost_LIBRARIES})

# look for Eigen
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

#configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake ${CMAKE_CURRENT_BINARY_DIR}/config.h)

add_subdirectory(cnn/cnn)
add_subdirectory(nt-parser)
# add_subdirectory(cnn/examples)
```


python2 preprocess.py /home/ji/code/rnng/wsj