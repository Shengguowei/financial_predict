1.针对原始数据，进行特征筛选：肉眼特征筛选，选出前三种；种类，现值，日期；

2.使用时间序列：pandas的一个数据结构，在上面可以直接对时间日期进行加减。

3.设定阈值：特色，不去预测数据的具体现值，预测数据可能超过某个阈值的。用前90天的数据去预测后三十天的数据。
  后三十天数据，在前90天数据的基础上进行优化。

4.调控阈值达到优化效果：如果我们提高召回率的阈值，那么精确率可以进一步的提高，因为需要的不是精确率，而是召回率和精确率


1.为了应付吕鹏的问题，开始更新的Readme

2018年7月1日

问题：2003.11.25输出结果错误。

1. 找到 nowday 在交易日列表中所在的索引，前取90个交易日索引，后取30个交易日索引
2. 对前90个交易日，取一个交易日，判断其是否存在于实际存在记录之中
         如果在：   计算其与 nowday 的相对净值
         如果不在：  缺失值添补
3. 对后30个交易日，取一个交易日，判断其是否在实际存在的记录之中
         如果在：   计算其与 nowday 的（now_value_behind - now_value)/now_value
         如果不在： 默认其值为0

对于2003.11.25这一天，会找到2004.1.6，其值为1.109，最后大于15%

综上，是sheng.Gw记忆出现出现偏差。

#### 2.模型的构造方式
    1.读取预处理完毕的中间文件
    2.对模型的两种比例，按照标签分割，分为标签为1，与标签为0，两类
    3.对数量较多的一种，进行随机采样，保持两类平衡
    4.将采样结果进行划分训练集和测试集
    5.将训练集扔到随机森林分类器中
    6.用测试集测试分类效果

#### 3.模型的输出方式
    1.输出的方式是将所有的数据集代入模型中，得到预测值和可能性
    2.将预测值和可能性输入文件中，文件名为 基金名+分类器名+probability.csv