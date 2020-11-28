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

python2 get_oracle.py data/train.all data/train.all > data/train.oracle 
python2 get_oracle.py data/train.all data/dev.all > data/dev.oracle 
python2 get_oracle.py data/train.all data/test.all > data/test.oracle
python2 get_oracle_gen.py data/train.all data/train.all > data/train_gen.oracle 
python2 get_oracle_gen.py data/train.all data/dev.all > data/dev_gen.oracle
python2 get_oracle_gen.py data/train.all data/test.all > data/test_gen.oracle


python2 preprocess.py /home/ji/code/brown-cluster/train.oracle  > /home/ji/code/brown-cluster/train.txt


./wcluster --text train.txt --c 156


python2 preprocess.py dev.oracle  > dev.stem
python2 preprocess.py test.oracle  > test.stem
 

./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/dev.stem -P -t --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 -D 0.2


TODO:
修改 rnng下的 get_oracle.py和 get_oracle_gen.py两个代码。因为数据中 nonterminal tokens种类较多，需要做一个 stemming的工作，将类似“NP-SBJ”这样的 nonterminal token -后部分去掉，变成“NP”。修改后的代码为get_oracle_stem.py和 get_oracle_gen_stem.py

all ->  oracle ->  stem 
gen.py