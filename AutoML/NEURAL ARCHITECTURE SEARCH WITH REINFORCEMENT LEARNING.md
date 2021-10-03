# Google提出的神经架构搜索
链接：[Google NAS](https://research.google/pubs/pub45826/)

## 1.动机
人工设计网络架构仍然有挑战，想要将其变成一个自动化的过程。

## 2.整体架构
基于RL的方法，用一个policy network（RNN）往外吐出network description

每次吐出的结果是softmax竞争的，因此我们可以推断，输出空间是预先设死的，在这个预设空间里搜索

在这些基础的设置上，文章玩花的，给出了如何使用这套框架generate出skip connection以及recurrent cell

## 3.借鉴思路
本质上是基于RL的sequence生成，policy network还可以吐别的东西，比如特征工程操作等