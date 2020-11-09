# AdaBoost(Adaptive Boosting)
## Abstract
1. 自适应提升算法，**自适应**代表泛化性强。**提升方法**指在分类问题中，改变Training Data的Weight来学习多个Classifiers，并进行线性组合提高性能
2. 思路：提高前几个「分类器线性组合」分错的样本的Weights，这样就可以让新的分类器可以更关注于「前面容易分错的Sample」上。最后的结果采用**加权投票**形式而非平均投票，这样可以让准确率较大的弱分类器有更大的权重[^1]
## Process
![[Pasted image 20201109155749.png]]
1. 给定一组Sample $\{(x_1,y_1),(x_2,y_2),...,(x_n,y_n)\}$
2. 假设第$k$次训练，第$i$个Sample的权重为$w_k^i$，第一次Classifier每个Sample的权重设为一样$w_1^i=1/n$
3. 假设我们要Sequential训练L个分类器，则重复以下步骤
4. For k = 1 -> k = L
	1. 用权重分布$w_k^i$训练弱分类器$f_k(x)$
	2. $\epsilon_k$为第$k$次分类器的Error rate
	3. $\alpha_k = 0.5 * ln((1-\epsilon_k)/\epsilon_k)$
	4. 根据第$k$次训练，第$i$个样本的权重，更新第$k+1$次训练，第$i$个Sample的权重$w_{k+1}^i = \begin{cases}w_k^i·e^{\alpha_k} & if·f_k(x_i)\neq y_i \\ w_k^i·e^{-\alpha_k} & if·f_k(x_i)=y_i\end{cases}$
		可以发现，如果预测正确，我们降低它的Weights，如果预测错误，我们升高它的Weights。从而让后面的分类器更加关注前面的分类器线性组合分错的部分。
## What's the value of $\alpha_k$
1. 可以发现，我们根据预测成功与否，针对第$k$个分类器的结果乘或者除一个系数$e^{\alpha_k}$，这个系数和$\alpha_k$紧密相关。
2. $\alpha_k$也是我们Reweighting Training Data的关键，其目的是为了让Error Rate = 0.5。具体细节同样参考下面的文章，以及视频[^2]
## References
[^1]: 機器學習: Ensemble learning之Bagging、Boosting和AdaBoost https://medium.com/@chih.sheng.huang821/%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-ensemble-learning%E4%B9%8Bbagging-boosting%E5%92%8Cadaboost-af031229ebc3
[^2]: ML Lecture 22: Ensemble https://www.youtube.com/watch?v=tH9FH1DH5n0