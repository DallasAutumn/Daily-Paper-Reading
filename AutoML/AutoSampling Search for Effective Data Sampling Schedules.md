# AutoSampling: Search for Effective Data Sampling Schedules

## 1.motivation
先前的一些方法都有一些人工先验：比如用RL的话，policy network的结构，数据集的一些固有条件比如噪声、不平衡性等

这篇文章想直接根据数据来选择采样策略，不借助这些先验信息

## 2.population-based training
本文使用了population-based training这一训练范式

这是hyper-parameter tuning任务中的一个并行训练范式。在$p$个worker上初始化权重完全相同的模型，设定$T$个时间步。每步记录下最好的那套超参，并且把训练出的模型权重copy到所有的worker上。下一步的超参每个worker再随机生成一套（文中用词是different random seeds），这相当于在整个超参空间中进行搜索。

但是文章通过举例说明，这种方法不能用于寻找采样策略，原因是会发生维度爆炸，计算成本太高。
## 3.framework
1. Exploration
   - 初始化一个均匀分布，用于采样，也就是说初始时每个数据点被等概率采到。这个采样分布每次通过exploitation传过来的reward来更新（实际上论文中的做法是直接用频率估计概率）。
   - 扰动这个采样分布，分配到$p$个worker上，相当于上述PBT训练范式的随机设置一套超参。论文中使用不同的随机种子来实现（真能省事）。
   - 每个worker根据拿到的分布采样一批数据，传给exploitation模块。
2. Exploitation
   - 每个worker把拿过来的数据用来训练子模型（这里叫子模型是因为p个worker并行，每个上面跑的模型都叫一个child model，只是单纯的叫法）
   - 每个worker计算一个reward传给exploration模块，让它去更新采样分布