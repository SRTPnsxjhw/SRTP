

# 理论1文本处理的NLTK使用方法：python自然语言处理
*主要是入门章节123+成分分析部分（main）+语义含义+语言数据管理（略过）*<br>
*主要研究NLTK库的使用方法*
### 第一章、第二章 Python和NLTK基本知识
* 搜索文本：similar(),common_contexts()
* 频率分布统计和高频词排序：dispersion_plot()，FreqDist()，sroted()...Page23
* python 的append连接方法-->连接词汇组成语句（英文）
* 词汇的搭配和双连词collocation() #TODO
* 断言判断(endwith & startwith)，即源码中的assert后的判断
* 一些语言理解技术：词意消除歧义，指代消解难题...
### 第三章 原始文本处理
<br>
<br>
<br>
<br>

### 第五章 分类和标注词汇
1. ##### 词性标注(Part-of-speech tagging)
   pos_tag(text)# 将一段文字进行分词性+词性标识组成不同的tuple，再进行list连接不同词性<br>

   ##CC(连词)  RB(副词)  IN(介词)  NN(名词)  JJ(形容词)balabala，P200可查<br>
****
   Q：相同词汇的词性不同用法<br>
   A: 使用pytorch进行模型建立和训练（？
   <br>
   <br>
****

1. ##### 分类断言：
   * 通过频率分布统计进行判定
   * 名词出现在限定词和形容词之后balabla一系列通过统计得到的判断（from布朗语料库）
   * 通过观察词前后的词性进行判断本词的词性tagged_words+FreqDist统计
2. ##### 自动标注的实现:
   - 词的标记依赖于其上下文+词本身的词性选择范围
   - 大多数新词都是名字-->默认标注为名词<br>  <br>
*在原本的词汇判断基础上简化新的低频词的判定方法*
  <br><br>
   -  正则表达式模型 #TODO
   -  最高频的部分词汇确定大量未标记词性的词汇的词性：
FreDist->UnigramTagger-->evaluate(排序-标记猜测-判断正确率)
   -  N-gram标注：
   - 单元->分离训练->多元
3. ##### 词的分类（语言学）

>就是对词汇(单or多（视为一组单）)贴标签，从布朗语料库中进行学习(训练)再推广到未见过的句子


# 理论2深度学习的实现方式：Python深度学习-基于Pytorch
### fundamental knowledge of Numpy and pytorch
环境安装+基本操作和函数使用，Tensor 和 Autograd好像我们用不上？
### 神经网络工具箱的使用
#TODO 未看
### Unit 7 NLP基础
#TODO 极晕，可能是神经网络部分未看原因
   



