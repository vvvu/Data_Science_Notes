# Ensemble Learning
## Definition
1. 集成学习：实际上相当于团队合作，集合**不同**的model进行预测[^1]
2. 这篇文章则给出了相似的定义[^2]，Ensemble learning, in general, is a model that makes predictions based on a number of different models. By combining individual models, the ensemble model tends to be more flexible (less bias) and less data-sensitive (less variance).
	- 这相当于一种解决Bias和Variance的Trade-off问题的一种解决方案

## Ensemble Methods
- [[Bagging]] - Training a buch of **individual** models in a parallel way. Each model is trained by a **random subset** of the data
- [[Boosting]] - Training a bunch of **individual** models in a **sequential way**. Each individual model **learns from mistakes made by the previous model.**

## The Difference Between Bagging & Boosting
|          | Bagging            | Boosting                                             |
| -------- | ------------------ | ---------------------------------------------------- |
| 样本选择 | 有放回抽样         | 训练集不变，训练集权重变化                           |
| 样例权重 | 均匀抽样，权重相等 | 根据错误率调整，错误率越大权重越大                   |
| 预测函数 | 预测函数权重相等   | 分类误差小的Classifier有更大的权重                   |
| 并行计算 | 可以并行           | 顺序生成，后一个Model Parameter需要前一个Model的结果 |




---

## References


[^1]: ML Lecture 22: Ensemble https://www.youtube.com/watch?v=tH9FH1DH5n0
[^2]: Basic Ensemble Learning (Random Forest, AdaBoost, Gradient Boosting)- Step by Step Explained https://towardsdatascience.com/basic-ensemble-learning-random-forest-adaboost-gradient-boosting-step-by-step-explained-95d49d1e2725