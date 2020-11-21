done: actions(t) utils(t)
fixme: example fields iterator + (t) 
todo: model / train


Alphabetical list of part-of-speech tags used in the Penn Treebank Project:
Number Tag Description
1.	CC	Coordinating conjunction
2.	CD	Cardinal number
3.	DT	Determiner
4.	EX	Existential there
5.	FW	Foreign word
6.	IN	Preposition or subordinating conjunction
7.	JJ	Adjective
8.	JJR	Adjective, comparative
9.	JJS	Adjective, superlative
10.	LS	List item marker
11.	MD	Modal
12.	NN	Noun, singular or mass
13.	NNS	Noun, plural
14.	NNP	Proper noun, singular
15.	NNPS	Proper noun, plural
16.	PDT	Predeterminer
17.	POS	Possessive ending
18.	PRP	Personal pronoun
19.	PRP$	Possessive pronoun
20.	RB	Adverb
21.	RBR	Adverb, comparative
22.	RBS	Adverb, superlative
23.	RP	Particle
24.	SYM	Symbol
25.	TO	to
26.	UH	Interjection
27.	VB	Verb, base form
28.	VBD	Verb, past tense
29.	VBG	Verb, gerund or present participle
30.	VBN	Verb, past participle
31.	VBP	Verb, non-3rd person singular present
32.	VBZ	Verb, 3rd person singular present
33.	WDT	Wh-determiner
34.	WP	Wh-pronoun
35.	WP$	Possessive wh-pronoun
36.	WRB	Wh-adverb
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
