# Deep Learning for Entity Matching: A Design Space Exploration

**EM**

Typically, EM is done in two phases: 

- blocking ：The goal of blocking is to filter the cross product D × D′ to a candidate set C that only includes pairs of entity mentions judged likely to be matches.

  - 相当于每两个entity都进行比较。

- matching： Given the labeled data T our goal is to design a matcher M that can accurately distinguish between “match” and “no-match” pairs (e1, e2) in C.

  - 判断pairs是否match

### 3.1 Architecture Template & Design Space

3.1 Architecture Template & Design Space

**输入input：**一对实体(e1,e2)，有相同的属性结构，属性值是a sequence of words we.j ，sequence的长度可以不同

每一个输入的点是n维向量，n为属性个数，第j维的值来自于两个点j维的值

![1564721287843](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564721287843.png)

​       

- **The Attribute Embedding Module：**For all attributes Aj ∈ A1 · · ·AN , this module takes sequences of words we1, j and we2, j and converts them to two sequences of word embedding vectors ue1, j and ue2, j whose elements correspond to d-dimensional embeddings of the corresponding words

  - 把属性转换为序列向量，转换为嵌入向量，1个元素为d维，一个属性有m个元素，就是d*m

  - 矩阵相乘，前面的列数==后面的行数

    ![1564728894906](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564728894906.png)

**输出output：**a pair of embeddings ue1, j and ue2, j
for the values of attribute Aj for entity mentions e1 and e2. We
denote the final output of this module as {(ue1, j , ue2, j )}N
j=1.

一对embeddings u

**属性相似度表示模型**

根据输入的u，判断e1和e2的相似度

1. 属性摘要

   把d*m和k维的进行软对齐，输出h维

2. 属性比较

   输入：h维

   输出：相似度sj  l维

   The output of the similarity representation module is a collection of similarity representation vectors {s1, . . . , sN }, one for each attribute A1, . . . ,AN

**The Classifier Module分类模块**

输入：s维

作为特征



![1564760870905](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564760870905.png)

**属性嵌入**

word-level or character-level

pre-trained or learned

**属性摘要**

1. 聚合函数：

平均 or 加权平均，不用学，依赖embedding向量

h=d

2. Sequence-aware Summarization序列感知的

用rnn，可考虑单词的顺序和语义

LSTM

缺点：长序列没有很好的表示

**Sequence Alignment属性对齐**

没有考虑到position

**Hybrid:混合**

时间多，但效果好

### Attribute Comparison Choices

距离计算

固定的：cosine    欧式距离

学习的