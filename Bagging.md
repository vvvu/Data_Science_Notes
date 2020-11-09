# Bagging
## Abstract
- 定义：**Bootstrap aggregating**, also called **bagging** (from **b**ootstrap **agg**regat**ing**)是一种Machine Learning方法，通过降低Variance来避免Overfitting
- 适用场景：用在较强的，不同于彼此的Model上。来解决Bias和Variance的Trade-off问题。在Model的Bias很小但是Variance较大的情况下，用Bagging来降低Variance从而**避免Overfitting**
- 为什么可以降低Variance：Bagging的核心步骤体现在**取样**这个过程。如果Training Data中有Noise，我们知道一般来讲Noise是比较小的，所以透过随机抽样我们就有机会让Noise不被训练到，所以可以降低Model的Variance
- Bootstrap Sample：下文提到的有放回的Sample方式我们称之为**Bootstrap Sample**，其定义可以见这里[^1]
## Process
![[Screen Shot 2020-11-09 at 13.37.07.png]]
1. 假设我们有N Training Examples，我们第一次从中采取**放回抽样(Resample)**抽取N'的子集样本$subset_1$。然后用N'的样本进行训练Model $f_1$
2. 放回数据，重复以上过程，采用放回抽样抽取子集样本$subset_i$，并用抽样得到的$subset_i$训练Model $f_i$。训练结束
3. 假设我们现在有经过n个子集训练得到的n个训练模型$f_1,...,f_n$。我们将Testing Data放入其中，每一个Model $f_i$都会产生一个结果$output_i$
		1. 对于Regression问题，我们对这个结果计算Average
		2. 对于Classification问题，我们对这个采取Voting策略
## Algorithms & Models
- [[Random Forest]]

---

## References
[^1]:https://en.wikipedia.org/wiki/Bootstrapping_(statistics)