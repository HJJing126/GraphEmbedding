# word embedding![1564734550147](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564734550147.png)

> https://www.bilibili.com/video/av56959021?from=search&seid=17230449452027579455

- one-hot

- bag of words  

  - <word, count> map
  - document similarity
    - cosine：夹角
    - 欧式距离
    - dot - product：点乘
    - tf-idf: 每个单词对这个文本的重要性且不是对素有文本都重要

- 神经网络

  - 单词相似或相关

    - 词性

    - 单词组合的频率高

      [girl, women ,boy,man]->[gender,age]

      任何东西都可以embedding

      

      

![1564734451378](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564734451378.png)

NNLM ：根据上文推测下文

上文：feture

单词：label

相似的单词在空间中更



## word2vec

CBOW

> 一个单词的含义不是由自己决定，而是上下文
>
> 每个词都是由相邻的词决定的



![1564734822692](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564734822692.png)

skip-gram

>  根据单词，预测上下文
>
> 每个词都决定了相邻的词

![1564734878540](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564734878540.png)

- huffman encoding and Hierarchical softmax

  权重大的词放前一点，减少运算量

- 负采样 negative Sampling

  we **play** game(neg,**eat**)

  算play的时候顺带把eat算了

  softmax：激励函数（sigmoid）归类问题

  ![1564735198176](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564735198176.png)

### GloVe

全局向量单词表示

上下文：全篇文章

不是滑动window，所以必须全部文章后才可以处理

### ELMo

2018

动态embedding：3个embedding的权重可调整



![1564735494655](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564735494655.png)

双层LSTM

![1564735577434](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564735577434.png)

效果很好

### BERT

双向 编码 表示 from transformers （特征提取器）替代RNN

大大提高了文本的特征提取能力，可以更好的找到单词的关系

从上下文预测word - cbow

- masked language model

训练过程中，随机去掉一些词，尽量让权重分散

![1564735884295](C:\Users\10344\AppData\Roaming\Typora\typora-user-images\1564735884295.png)



TensorFlow hub

bert-as- service

推荐uncased

### 万物皆可embedding

- 用一个低维的向量表示一个物体，可以是一个词，或是一个商品，或是一个电影等等
- 这个embedding向量的性质是能使距离相近的向量对应的物体有相近的含义
- Embedding甚至还具有数学运算的关系
  - 比如Embedding（马德里）-Embedding（西班牙）+Embedding(法国)≈Embedding(巴黎)

### word2vec



### Graph Embedding

> https://blog.csdn.net/Yasin0/article/details/90313313

#### deepwalk

随机游走，将点的序列表示成文本

改进：根据概率游走

**阿里论文**

主要思想：在由物品组成的图结构上进行随机游走，产生大量物品序列，然后将这些物品序列作为训练样本输入word2vec进行训练，得到物品的embedding

![img](https://img-blog.csdnimg.cn/20190518114637829.png)

b. 若产生了多条相同的有向边，则有向边的权重被加强。在将所有用户行为序列都转换成物品相关图中的边之后，全局的物品相关图就建立起来了。

3.图c采用随机游走的方式随机选择起始点，重新产生物品序列。

4.图d最终将这些物品序列输入word2vec模型，生成最终的物品Embedding向量。

随机游走的跳转概率，也就是到达节点vi后，下一步遍历vi的临接点vj的概率。如果物品的相关图是有向有权图，那么从节点vi跳转到节点vj的概率定义如下：

![img](https://img-blog.csdnimg.cn/20190518114710294.png)

权重Mij/点i的所有出边的权重之和

**Node2vec**

**同质性：**距离相近节点的embedding应该尽量近似

eg. 节点u与其相连的节点s1、s2、s3、s4

**结构性：**结构上相似的节点的embedding应该尽量接近，

eg. 节点u和节点s6

![img](https://img-blog.csdnimg.cn/2019051811474671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lhc2luMA==,size_16,color_FFFFFF,t_70)

控制bfs和dfs的概率

![img](https://img-blog.csdnimg.cn/20190518114815763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lhc2luMA==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/20190518114839403.png)

![img](https://img-blog.csdnimg.cn/20190518114939849.png)

dtx指的是节点t到节点x的距离

参数p和q共同控制着随机游走的倾向性。

参数p被称为返回参数（return parameter），p越小，随机游走回节点t的可能性越大，node2vec就更注重表达网络的同质性，

参数q被称为进出参数（in-out parameter），q越小，则随机游走到远方节点的可能性越大，node2vec更注重表达网络的结构性，反之，当前节点更可能在附近节点游走。

**同质性相同的物品很可能是同品类、同属性、或者经常被一同购买的物品**

**结构性相同的物品则是各品类的爆款、各品类的最佳凑单商品等拥有类似趋势或者结构性属性的物品**。

> 阿里的Graph Embedding方法EGES

2018年阿里公布了其在淘宝应用的Embedding方法EGES（Enhanced Graph Embedding with Side Information），其基本思想是在DeepWalk生成的graph embedding基础上引入补充信息。

新加入的物品，或者没有过多互动信息的长尾物品，推荐系统将出现严重的**冷启动问题**。为了使“冷启动”的商品获得“合理”的初始Embedding，阿里团队通过引入了更多**补充信息**来丰富Embedding信息的来源，从而使没有历史行为记录的商品获得Embedding。

生成Graph embedding的第一步是生成物品关系图，通过用户行为序列可以生成物品相关图，利用相同属性、相同类别等信息，也可以通过这些相似性建立物品之间的边，从而生成**基于内容的knowledge graph**。而基于knowledge graph生成的物品向量可以被称为补充信息（side information）embedding向量，当然，根据补充信息类别的不同，可以有多个**side information embedding向量**。

那么如何融合一个物品的多个embedding向量，使之形成物品最后的embedding呢？最简单的方法是在深度神经网络中加入average pooling层将不同embedding平均起来，阿里在此基础上进行了加强，对每个embedding加上了权重，如图7所示，对每类特征对应的Embedding向量，分别赋予了权重a0，a1…an。图中的Hidden Representation层就是对不同Embedding进行**加权平均**操作的层，得到加权平均后的Embedding向量后，再直接输入softmax层，这样通过梯度反向传播，就可以求的每个embedding的权重ai(i=0…n)。

![img](https://img-blog.csdnimg.cn/20190518115058979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1lhc2luMA==,size_16,color_FFFFFF,t_70)

