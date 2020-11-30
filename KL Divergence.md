# KL Divergence
一句话总结：衡量两个Probability Distribution之间的差异，值越接近于0两个Probability Distribution越相似。$KL\text{ }Divergence \geq 0$
## Introduction
1. Kullback-Leibler Disvergence，即我们常说的KL散度。又名相对熵**Relative Entropy**，信息散度**Information Divergence**[^1]
2. 主要特性如下
	- 是两个概率分布Probability Distribution之间差异的**非对称性度量**。在信息理论中，KL散度等于两个Probability Distribution的信息熵**Shannon Entropy**的差值
	- 是一些优化算法的Loss Funciton，例如其为[[Expectation-Maximization Algorithm]]算法的Loss Function => 此时两个概率分布分别为真实分布和拟合分布，KL散度可以哦你过来计算拟合分布拟合真实分布时的信息损耗

## Definition
设$P(x),Q(x)$是随机变量$X$的两个Probability Distribution
- 离散情况下KL Divergence的定义为
$$
KL(P||Q) = \sum P(x)log\frac{P(x)}{Q(x)}
$$
- 连续情况下KL Divergence的定义为
$$
KL(P||Q) = \int P(x)log\frac{1}{Q(x)}dx
$$

推导：KL Divergence是两个Probability Distribution的Shannon Entropy的差值
$$
KL(P||Q) = -\sum_{x \in X}P(x)log\frac{1}{P(x)} + \sum_{x \in X}P(x)log\frac{1}{Q(x)} = \sum_{x \in X}P(x)log\frac{P(x)}{Q(x)}
$$
其中**Shannon Entropy**的定义为
$$
H(x) = -\sum_{x \in X}P(x)log\frac{1}{Q(x)}
$$

## Why We Need KL Divergence ?
1. 假设我们有一个**真实Distribtuion**，我们想要用已知的Distribution去**拟合**它
2. 则我们需要对**已有Distribution**进行不断调参，KL Divergence可以衡量两个分布之间的差异，所以**也可以用来衡量已有Distribtuion拟合真实Distribtuion的效果如何**
3. KL Divergence的值**越低越好**[^2]，当$P(x) == Q(x)$时
$$
KL(P||Q) = \sum P(x)log\frac{P(x)}{Q(x)} = \sum P(x)log1 = 0
$$

## KL Divergence && Cross Entropy
我们可以通过KL Divergence来推导得到[[Cross Entropy]]
假设我们有两个Probability Distribution $p(x),q(x)$,我们定义H(p,q)为Cross Entropy, $D_{KL}(p|q)$为KL Divergence
- Cross Entropy的定义如下
$$
H(p,q) = -\sum_xp(x)logq(x)
$$
- KL Divergence的定义如下
$$
D_{KL}(p|q) = \sum_xp(x)log\frac{p(x)}{q(x)}
$$
- 推导得
$$
D_{KL}(p|q) = \sum_xp(x)log\frac{p(x)}{q(x)} =\sum_x(p(x)logp(x) - p(x)logq(x))
$$
$$
= -H(p,p)-\sum_xp(x)logq(x) = -H(p,p) + Hp,q)
$$
而根据Cross Entropy的定义，$H(p,p) = H(p)$
所以
$$
H(p,q) = D_{KL}(p|q) + H(p)
$$
**1. 综上所述，KL散度 = 交叉熵 - 熵，这就是KL散度和交叉熵之间的关系。**
**2. 我们已知两个Distribution的KL散度越低，拟合的越好（两个越接近），所以我们希望KL散度越低越好**
**3. 又因为，对于给定的Training Set这个Distribution，H(p)熵是已知的，所以求KL散度等价于求交叉熵（两者此时只隔着一个常数H(p)而已），所以Cross Entropy常被用作Loss Function**

[^1]:https://baike.baidu.com/item/%E7%9B%B8%E5%AF%B9%E7%86%B5/4233536?fromtitle=KL%E6%95%A3%E5%BA%A6&fromid=23238109&fr=aladdin
[^2]:https://zhuanlan.zhihu.com/p/37452654