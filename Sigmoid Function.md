# Sigmoid Function
## Definition
Sigmoid函数（又名Logisitc函数）的定义为
$$
\sigma(a) = \frac{1}{1+e^{-a}}
$$

---

**Bayes's Theorem推导过程如下**：
1. 首先考虑二分类的情况，类别为$C1,C2$。对于 $x$ 分类到$C1$的后验概率根据[[Bayes's Theorem]]可以写成
$$
p(C1|x) = \frac{p(x|C1)p(C1)}{P(x|C1)p(C1)+p(x|C2)p(C2)}
$$
2. 其中我们可以将该公式右边上下同除$p(x|C1)p(C1)$，可以得到
$$
P(C1|x) = \frac{1}{1 + \frac{p(x|C2)p(C2)}{p(x|C1)p(C1)}}
$$
3. 我们可以定义
$$
z = ln(\frac{p(x|C1)P(C1)}{p(x|C2)p(C2)})
$$
4. 则原公式写作
$$
p(C1|x) = \frac{1}{1+e^{-z}}
$$
5. 我们可以定义$\sigma(z) = \frac{1}{1+e^{-a}}$
6. 则原公式实际上为
$$
P(C1|x) = \sigma(z)
$$

---

**特点：**
1. 平滑曲线，易于求导
2. 导函数可以用自身表示 $\sigma^{'}(x) = \sigma(x)(1-\sigma(x))$

---

**为什么我们需要Sigmoid函数：**
这段内容主要根据这篇文章[^1]进行推导。

为什么我们需要Sigmoid函数而不是直接使用Bayes函数：
- Bayes有个限制条件是所有的Feature都必须是Independent的，这样才可以构成一个样本空间的**全划分**。但是在实际训练中不太可能，此时时候Bayes导致的Bias就会非常大，Model效果不佳

这个$\sigma(z)$中的$z$应该是什么样子的？
1. 将$z$变换一下得到$$z = ln\frac{P(x|C1)}{p(x|C2)} + ln\frac{P(C1)}{P(C2)}$$
2. 其中$P(C1)/P(C2)$非常容易求解，假设$C1$类别在Training_Data中出$N_1$次，$C2$类别出现$N_2$次。则$$ln\frac{P(C1)}{P(C2)} = ln\frac{\frac{N_1}{N_1+N_2}}{\frac{N_2}{N_1+N_2}} = ln\frac{N_1}{N_2}$$
3. 而$P(x|C1)$和$P(x|C2)$遵循 [[Gaussian Probability Distribution#Multivariate Bivariate Gaussian Distribution]]
	我们认为两个Class的协方差矩阵，即$\Sigma_1 = \Sigma_2 = \Sigma$ ![[Pasted image 20201105170902.png]]。原因参照这这篇文章[^2]，**「如果我们协方差矩阵不相等，那我们的决策边界（超平面）是抛物线的，只有在想等的情况下才是线性的」**
	综上，因为$\Sigma_1 =\Sigma_2$，则可以将$z$化简为如下形式$$z = (\mu_1-\mu_2)\Sigma^{-1}x - \frac{1}{2}\mu_1^T\Sigma^{-1}\mu_1+\frac{1}{2}\mu_2^T\Sigma^{-1}\mu_2 + ln\frac{N_1}{N_2}$$
	实际上这本身就是一个线性函数，前面的部分为$x$向量的系数，后面的部分为bias
4. 综上，我们的目标就是寻找最佳的$N_1,N_2,\mu_1,\mu_2,\Sigma$让$P(C1|x)$最大化



---
## References

[^1]:https://zhuanlan.zhihu.com/p/108641430
[^2]:https://towardsdatascience.com/why-sigmoid-a-probabilistic-perspective-42751d82686