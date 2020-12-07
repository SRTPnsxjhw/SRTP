DONE: actions(t) utils(t)
TODO: example fields iterator + (t)  and   model / train
TODO: evalb executable file / params file

TODO：更改oracle.cc
  train为训练语料，用于模型训练；
  dev为开发集，用于模型参数调优；
  test用于测试
  
Alphabetical list of part-of-speech tags used in the Penn Treebank Project:
Number Tag Description
ROOT：要处理文本的语句

IP：简单从句
NP：名词短语
VP：动词短语
PU：断句符，通常是句号、问号、感叹号等标点符号
LCP：方位词短语
PP：介词短语
CP：由‘的’构成的表示修饰性关系的短语
DNP：由‘的’构成的表示所属关系的短语
ADVP：副词短语
ADJP：形容词短语
DP：限定词短语
QP：量词短语
NN：常用名词
NR：固有名词
NT：时间名词
PN：代词
VV：动词
VC：是
CC：表示连词
VE：有
VA：表语形容词
AS：内容标记（如：了）
VRD：动补复合词
CD: 表示基数词
DT: determiner 表示限定词
EX: existential there 存在句
FW: foreign word 外来词
IN: preposition or conjunction, subordinating 介词或从属连词
JJ: adjective or numeral, ordinal 形容词或序数词
JJR: adjective, comparative 形容词比较级
JJS: adjective, superlative 形容词最高级
LS: list item marker 列表标识
MD: modal auxiliary 情态助动词
PDT: pre-determiner 前位限定词
POS: genitive marker 所有格标记
PRP: pronoun, personal 人称代词
RB: adverb 副词
RBR: adverb, comparative 副词比较级
RBS: adverb, superlative 副词最高级
RP: particle 小品词 
SYM: symbol 符号
TO:”to” as preposition or infinitive marker 作为介词或不定式标记 
WDT: WH-determiner WH限定词
WP: WH-pronoun WH代词
WP$: WH-pronoun, possessive WH所有格代词
WRB:Wh-adverb WH副词
https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html



Pretrained Generative Model
As the generative model takes a while to train, a pretrained model is available here: (version as of Apr 16 2018)
Since CNN/DyNet relies on Boost to serialize and save/load the model, using this pretrained model requires using boost version 1.60.0 to compile the system. It is important to specify the same training set and clusters in order to load the model (otherwise the model cannot be loaded). Here is the command for training the pretrained model:
build/nt-parser/nt-parser-gen -x -T /usr1/home/akuncoro/rnng-dataset/english/train-gen.oracle -d /usr1/home/akuncoro/rnng-dataset/english/dev-gen.oracle -t --clusters /usr1/home/akuncoro/rnng-dataset/english/clusters-train-berk.txt --input_dim 256 --lstm_input_dim 256 --hidden_dim 256 -D 0.3
The clusters that we used is the "clusters-train-berk.txt", and please contact us to get access to the oracle due to PTB licensing issues.

Pretrained Discriminative Model Tree Samples For Reranking
To get tree samples from the discriminative models (in order to use the generative model to rescore these samples), please find "test-samples.props" here:  
这个readme写的好乱鸭，我的理解是：parsing_result.txt似乎是这个工程的最终目标，利用预训练模型的时候，我们需要四个文件：train-gen.oracle、dev-gen.oracle、clusters-train-berk.txt和pretrained-gen-new.params；其中，前两个文件需要联系作者索取，第三个由第一个生成。有了这四个文件，直接执行readme中的Generative Model（相应指令用Pretrained Generative Model中的代替）、Decoding with the generative model这两大部分就行了。
train-gen.oracle、dev-gen.oracle文件
