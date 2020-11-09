# Decision Tree

## Decision Tree Consists of
1. **Nodes**
2. **Edges | Branch**
3. **Leaf Nodes** - 输出结果（可能是Class Label，也可能是Class Distribution）

---

## Two Types of Decision Tree
1. Classification Tree - 分类树
2. Regression Tree - 回归树

### Classification Tree
![[Screen Shot 2020-11-09 at 19.34.52.png]]
1. 主要针对**分类问题**，故输入数据也是 **Discrete Data**
2. 输出结果为**Class Label**，即特定的分类结果，表明输入的数据归属于哪一个类别

### Regression Tree
![[Screen Shot 2020-11-09 at 19.41.09.png]]
1. 主要针对回归问题，故输入数据也是 **Continuous Data**
2. 输出结果为连续值，而非离散的分类类别。

---

## How to Build a Decision Tree
建立Dicision Tree的重点在于**split**过程，不过算法发生**split**的时间点并不相同
### 1. ID3 (Iterative Dichotomiser 3)
1. Metrics : **Entropy Function** and **Information Gain**
2. 已知信息熵越大，样本纯度越低 -> ID3的核心思想就是选择**Information Gain**最大的Feature进行**split**
	- Information Gain 表示Feature A使Sample不确定性减少的程度， 以Information Gain最大的Feature进行Split可以让不确定性下降最快，**从而让样本纯度提升越高**，这样就可以让一类事物在同一个Space中
	- 总而言之：Information Gain越大表示用Feature A来Split所获得的“Sample纯度提升越大”

### 2. C4.5
1. Metrics : Normalized Information Gain
2. 使用**信息增益率**作为分裂标准，克服了ID3 对特征数目的偏重这一特点

### 3. CART
1. Metrics : Gini
2. 使用Gini克服C4.5需求$log$的巨大计算量

### 4. Difference & Summary
|          | ID3                              | C4.5                                                   | CART                                                         |
| -------- | -------------------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| 划分标准 | 使用信息增益，偏向特征值多的特征 | 使用信息收益率，克服信息增益的缺点，偏向特征值少的特征 | 使用基尼系数，克服C4.5需要求对数的计算量，偏向于特征值多的特征 |
| 适用场景 | 分类问题                         | 分类问题                                               | 分类 & 回归                                                  |
| 样本数据 | 仅离散数据，缺失值敏感           | 连续性数据，多种方式处理缺失值                         | 连续性数据，多种方式处理缺失值                               |
| 样本规模 |                                  | 小样本                                                 | 大样本                                                       |
| 样本特征 | 只使用一次                       | 只使用一次                                             | 多次重复使用                                                 |
| 剪枝策略 | 无                               | 悲观剪枝策略                                           | 代价复杂度剪枝                                               |


