# Random Forest
## Abstract
- [[Decision Tree]]容易**Overfitting**，且如果我们不限制Decision Tree的Depth，我们最终可以得到Error rate = 0的Decision Tree Model
- Random Forest采用了[[Bagging]]技术来解决Decision Tree所遇到的问题。所以又名**Random Forest : Bagging of Decision Tree**，而Random Forest的全名也可以叫作**Random Decision Forest**，可以看出两者之间的关系。
- 为什么叫Forest？ - Forest由许多**互不相关的Decision Tree**构成，实际上Decision Tree的个数也是Random Forest的重要Parameters之一

## Process
![[Pasted image 20201109133438.png]]
1. 第一步：采用Bootstrap Sample技术（即有放回抽样）从Entire Dataset中进行采样
2. 训练**n**棵Decision Tree - 两个随机性
	- 随机性（1）：一个Random Subset只能用来训练一个Decision Tree
	- 随机性（2）：如果每个Sample有M features，我们指定常数m << M，**随机**从M features中选择m个features subsets，剩余的M - m features则不在这次训练中使用。「这样即使Dataset是一样的，产生的Decision Tree也是不同的」
3. **n**棵Decision Tree**独立**预测结果
4. 根据 Regression | Classification 来判断输出 Final Prediction
	- Regression - Average
	- Classification - Majority Vote

---

## Out-of-bag Validation
1. **Bagging**，也是**Random Forest**的交叉验证方式
2. 在前文训练$f_i$时，每次我们都会取Entire Dataset N中的子集Subset N'，针对每个Model $f_i$，我们以N'为Training Data，N-N'为Validation Set，算出所有Model $f_i$的error-rate，最后取Average

---

## Advantages & Disadvantages
### 1. Advantages
- 可以用来解决 Classification | Regression 问题，**可以同时处理Numeric data and Discrete data**
- 防止Overfitting：通过平均Decision Tree，降低Variance -> 降低Overfitting
- 非常稳定：只有半数以上的Decision Tree出现错误时才会有错误的预测

### 2. Disadvantages
- 复杂，计算成本高
- 复杂，训练时间久

---

## Hyper-parameters
1. 提高预测能力
	- Decision Tree的数量：数量越多，性能越高，结果越稳定，计算速度越慢
	- 节点分裂(split)时参与判断的最大特征数：Random Forest允许单个Decision Tree使用Features的最大数量
	- 叶子结点最小样本数：内部结点再划分所需最小样本数
2. 加快建模速度
	- 并行数：允许使用处理器的数量
	- 随机数生成器：每次产生不同的结果
	- 是否计算Out-of-bag validation：Random Forest的交叉验证方法