# Anomaly Detection
## Introduction
- Anomaly在中文中被翻译为**异常**，偏贬义。但在原语境中是一个**中性词**
- Anomaly Detection并非一定是要找出Negative的东西，而只是找出和Normal Data不一样的东西

## Applications
- Fraud Detection：正常刷卡行为 | 盗刷，相关Kaggle竞赛[^1][^2]
- Network Intrusion Detection：正常连接 | 攻击行为，相关资料[^3]
- Cancer Detection：正常细胞 | 癌细胞，相关Kaggle竞赛[^4]

## Metrics
Confidence Score - 信心分数，表示Model对自己的预测结果的信心。在Anomaly Detection中，我们可以认为当Confidence Score超过某个特定的值Threshold $\lambda$时，为Normaly，当Confidence Score低于Threshold时，我们认为是Anomaly
$$
f(x) = \begin{cases} normal, &confidence(x) > \lambda \\ anomaly, & confidence(x) \leq  \lambda \end{cases}
$$

1. **How to estimate Confidence** => 不同的评估方法
   - The maximum scores - Model分类结果的概率最高值作为Confidence Score
   - Negative Entropy - Model分类结果的概率比较平均，说明Model无法确认结果应该分到哪一类，此时Confidence Score就会很低
2. **Dev Set (Development Set | Validation Set)** => To determine $\lambda$ and other hyperparameters 

## The Differences Between Anomaly Detection and Binary Classification
- Binary Classification问题中，我们是知晓两种类别的特征。比如说我们分类🐱和🐶，我们显然是知道🐱和🐶的各自特征，所以我们可以进行正确的分类
- Anomaly Detection问题中，我们可能会遇到我们此前从未见过的生物。例如我们假设🐱是Normal的，我们的训练集里大部分就都是🐱。这时候在测试集中，让我们的Model去面对🐶或者其他动物，实际上之前Model是没有见过这个生物的。则如何让Model将这种生物分类到Anomaly是一个非常关键的问题

## Popular Techniques
1. 最前面提到的Classifier，利用如下公式
   $$
   f(x) = \begin{cases} normal, &confidence(x) > \lambda \\ anomaly, & confidence(x) \leq  \lambda \end{cases}
   $$
2. 利用GAN解决网络来解决样本不平衡问题
	- 样本不平衡问题：Normal和Anomaly样例的比例是极其不平衡的 => 所以我们不能使用Accuracy来评估Model的好坏
	- Metrics - For example, **Area under ROC Curve**
	- 很多时候很难收集到Anomaly Data => **Generating by Generative Models**，用生成的Anomaly Data来训练Classifers
3. GMM (Gaussian Mixture Model) => 1-dim | 2-dim | N-dim
4. Auto-encoder - 整体流程为Training data -> Encoder -> code -> Decoder -> Training data
   1. 如果是Normal的图片，在经过Encode和Decode后，加密解密过的结果应该是非常相像的
   2. 如果是Anomaly的图片，在经过Encode和Decode后，最后的结果会差异很大，很难被还原
   总之：Auto-encoder非常擅长加密解密还原Normal的数据，并不擅长加密解密还原Anomaly的数据
5. PCA
6. One- class SVM[^5]
7. Isolated Forest[^6]

## Difference Aspects of Anomaly Detection
- [[Time Series Anomaly Detection]]
- [[Anomaly Detection On Audio]]
- [[Anomaly Detection On Image]]

### References

[^1]: https://www.kaggle.com/ntnu-testimon/paysim1/home
[^2]: https://www.kaggle.com/mlg-ulb/creditcardfraud/home
[^3]: http://kdd.ics.uci.edu/databases/kddcup99/kddup99.html
[^4]: https://www.kaggle.com/uciml/breast-cancer-wisconsin-data/home
[^5]: https://papers.nips.cc/paper/1723-support-vector-method-for-novelty-detection.pdf
[^6]: https://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/icdm08b.pdf?q=isolation-forest