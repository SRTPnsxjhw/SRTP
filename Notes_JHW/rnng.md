# 环境配置

/home/ji/code/rnng

g++ -o test test.cc -I /home/ji/code/boost_1_61_0 -L /home/ji/code/boost_1_61_0/stage/lib -no-pie

SET (BOOST_ROOT "/home/ji/code/boost_1_61_0") 
SET (Boost_INCLUDE_DIR "/home/ji/code/boost_1_61_0") 
SET (Boost_LIBRARIES "/home/ji/code/boost_1_61_0/stage/lib") 
SET (EIGEN3_INCLUDE_DIR "/home/ji/code/eigen")

```
make文件
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

# 预处理
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
 
TODO:
修改 rnng下的 get_oracle.py和 get_oracle_gen.py两个代码。因为数据中 nonterminal tokens种类较多，需要做一个 stemming的工作，将类似“NP-SBJ”这样的 nonterminal token -后部分去掉，变成“NP”。修改后的代码为get_oracle_stem.py和 get_oracle_gen_stem.py

all ->  oracle ->  stem 
gen.py

# Train Disc(-P)

## POS_tag
./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/dev.stem -P -t --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 -D 0.2

./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/test.stem -m latest_model -P -p data/test.oracle --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 D 0.2


## not POS_tag

./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/dev.stem  -t --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 -D 0.2

./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/test.stem -m ntparse_0_2_128_128_16_128-pid2177.params  -p data/test.oracle --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 D 0.2

# Decoding Disc
```
Configuration options:
  -T [ --training_data ] arg          List of Transitions - Training corpus
  -x [ --explicit_terminal_reduce ]   [recommended] If set, the parser must 
                                      explicitly process a REDUCE operation to 
                                      complete a preterminal constituent
  -d [ --dev_data ] arg               Development corpus
  -C [ --bracketing_dev_data ] arg    Development bracketed corpus
  -p [ --test_data ] arg              Test corpus
  -D [ --dropout ] arg                Dropout rate
  -s [ --samples ] arg                Sample N trees for each test sentence 
                                      instead of greedy max decoding
  -a [ --alpha ] arg                  Flatten (0 < alpha < 1) or sharpen (1 < 
                                      alpha) sampling distribution
  -m [ --model ] arg                  Load saved model from this file
  -P [ --use_pos_tags ]               make POS tags visible to parser
  --layers arg (=2)                   number of LSTM layers
  --action_dim arg (=16)              action embedding size
  --pos_dim arg (=12)                 POS dimension
  --input_dim arg (=32)               input embedding size
  --hidden_dim arg (=64)              hidden dimension
  --pretrained_dim arg (=50)          pretrained input dimension
  --lstm_input_dim arg (=60)          LSTM input dimension
  -t [ --train ]                      Should training be run?
  -w [ --words ] arg                  Pretrained word embeddings
  -b [ --beam_size ] arg (=1)         beam size
  -h [ --help ]                       Help
```
## P
./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/mytest.stem -m ntparse_pos_0_2_128_128_16_128-pid62654.params -P -p data/mytest.oracle --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 D 0.2 > data/output.txt

## not_P
./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/mytest.stem -m ntparse_0_2_128_128_16_128-pid2177.params  -p data/mytest.oracle --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 D 0.2 > data/output_nop.txt

## Test input 
```
# (S (NP-SBJ-1 (NP (DT The) (NN economy) (POS 's) ) (NN temperature) ) (VP (MD will) (VP (VB be) (VP (VBN taken) (NP (-NONE- *-1) ) (PP-CLR (IN from) (NP (JJ several) (NN vantage) (NNS points) )) (NP-TMP (DT this) (NN week) ) (, ,) (PP (IN with) (NP (NP (NNS readings) ) (PP (IN on) (NP (NP (NN trade) ) (, ,) (NP (NN output) ) (, ,) (NP (NN housing) ) (CC and) (NP (NN inflation) )))))))) (. .) )
DT NN POS NN MD VB VBN -NONE- IN JJ NN NNS DT NN , IN NNS IN NN , NN , NN CC NN .
The economy 's temperature will be taken *-1 from several vantage points this week , with readings on trade , output , housing and inflation .
the economy 's temperature will be taken *-1 from several vantage points this week , with readings on trade , output , housing and inflation .
The economy 's temperature will be taken *-1 from several UNK-LC points this week , with readings on trade , output , housing and inflation .
NT(S)
NT(NP-SBJ-1)
NT(NP)
SHIFT
SHIFT
SHIFT
REDUCE
SHIFT
REDUCE
NT(VP)
SHIFT
NT(VP)
SHIFT
NT(VP)
SHIFT
NT(NP)
SHIFT
REDUCE
NT(PP-CLR)
SHIFT
NT(NP)
SHIFT
SHIFT
SHIFT
REDUCE
REDUCE
NT(NP-TMP)
SHIFT
SHIFT
REDUCE
SHIFT
NT(PP)
SHIFT
NT(NP)
NT(NP)
SHIFT
REDUCE
NT(PP)
SHIFT
NT(NP)
NT(NP)
SHIFT
REDUCE
SHIFT
NT(NP)
SHIFT
REDUCE
SHIFT
NT(NP)
SHIFT
REDUCE
SHIFT
NT(NP)
SHIFT
REDUCE
REDUCE
REDUCE
REDUCE
REDUCE
REDUCE
REDUCE
REDUCE
SHIFT
REDUCE
```
# Train Gen
./build/nt-parser/nt-parser-gen -x -T data/train_gen.oracle -d data/dev_gen.oracle -c data/word_clusters.txt -t --input_dim 256 --lstm_input_dim 256 --hidden_dim 256 -D 0.3

## Decoding Gen 

## 从discriminative model 中采样
./build/nt-parser/nt-parser -x -T data/train.oracle -d data/dev.oracle -C data/test.stem -m ntparse_pos_0_2_128_128_16_128-pid62654.params -P -p data/test.oracle --input_dim 128 --lstm_input_dim 128 --hidden_dim 128 -D 0.2 -s 100 -a 0.8 > test-samples.props

 
## 去除test-samples.props 中每一行的多余部分
utils/cut-corpus.pl 3 test-samples.props > test-samples.trees
 
## 从生成模型中获得联合概率 6h左右
./build/nt-parser/nt-parser-gen -x -T data/train_gen.oracle --clusters data/word_clusters.txt --input_dim 256 --lstm_input_dim 256 --hidden_dim 256 -p test-samples.trees -m ntparse_gen_D0.3_2_256_256_16_256-pid3260.params > test-samples.likelihoods
 
## 获得边缘概率

utils/is-estimate-marginal-llh.pl 2416 100 test-samples.props test-samples.likelihoods > llh.txt 2> rescored.trees


## 最后

utils/add-fake-preterms-for-eval.pl rescored.trees > rescored.preterm.trees 
utils/replace-unks-in-trees.pl data/test.oracle rescored.preterm.trees > hyp.trees
python2 utils/remove_dev_unk.py data/test.stem hyp.trees > hyp_final.trees 
EVALB/evalb -p EVALB/COLLINS.prm data/test.stem hyp_final.trees > parsing_result.txt

## 参数
llx.txt 文件中的最后几行会给出language model 中边缘概率p(x)的perplexity， 大
约可以达到 88.66。parsing_result.txt 中给出了generative model 的准确率，F1 值大约为
92.88。


