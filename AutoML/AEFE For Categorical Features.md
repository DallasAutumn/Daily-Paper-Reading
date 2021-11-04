# Auto Embedded Feature Engineering For Categorical Features

## 1.Motivation
广告推荐场景中的任务（如CTR）经常需要引入离散特征的交叉组合，比如：某用户在过去10天内购买的商品，这样的特征是基于一些基本的如时间戳，商品名等特征组合而成，SQL数据库中由于范式要求，不会存储这么复杂的特征，一般需要算法工程师手动构建出来。

组合特征高度稀疏，用one-hot直接维度爆炸废了，因而出现了Factorization Machine（FM）这样做embedding的模型。但是这样的深度嵌入（如DeepFM）又缺乏可解释性。

本文的framework：
- Automatic，自动进行特征构造和选择
- 能够生成丰富的meaningful的统计特征
- 引入了一些优化技术来提升性能和拓展性
- 构建出的特征加上GBDT模型，可超过深度学习模型的SOTA效果

## 2.Framework
1. Construction

    采用的特征变换操作：
    - 分组（GroupBy）
    - 聚合（Aggregate）
    - 组合（Combine）
    能够组合出“什么行业的用户过去几天里平均浏览商品多少次“这样的复杂特征

2. Selection
    - Filter：设一个方差的阈值，过滤方差小于该阈值的特征
    - Embedded：用能够计算特征权重的树模型之类的（比如GBDT，RF）
    - Wrapper：逐个加入特征进行训练
