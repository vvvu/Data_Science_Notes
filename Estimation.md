# Estimation
MLE - [[Maximum Likelihood Estimate]]
MAP - [[Maximum A Posteriori Probability]]
BE - [[Bayesian Estimation]] 

---

## Introduction
1. 为什么要进行**Estimation** - 通过观察到的观测值来反向推理出随机变量的相关性质，**如$E,Var$甚至是分布本身**
2. 参数估计 & 非参数估计
	1. 参数估计 - 对于估测值，我们已知它服从某种分布或假设它服从某种分布
	2. 非参数估计 - 对于估测值，我们并不知道样本服从的分布
3. 频率学派和贝叶斯学派
>**频率学派** 的学者认为随机变量分布的参数虽然未知，但是不会改变，是**固定的常数**
>**贝叶斯学派** 的学者认为随机变量分布的参数是随机的，我们无法得到参数的具体值，**参数也服从分布**

## Frequentist & Bayesian
**不同学派虽然有分歧，但解决问题的本质是求参数$\theta$**

1. Frequentist - 对于一个抛硬币问题，如果抛一枚硬币100次，20次朝上，频率学派会估计$\theta = \frac{20}{100} = 0.2$，很直观，当**数据量趋于无穷时可以给出精确的估计**，然后在缺乏数据时会有很大的误差。例如，如果抛5次硬币全部朝上，则$\theta = 5/5 = 1$，出现了严重的误差

2. Bayesian - 在Bayesian学派中有两大输入和一大输出

   - 两大输入：先验$P(\theta)$和似然$P(X|\theta)$。
     - 先验：$P(\theta)$ 指没有观测到任何数据时对$\theta$的预先判断，例如给我一枚硬币，我有一种先验认为这个硬币很大概率是均匀的
     - 似然：$P(X|\theta)$，是假设 $\theta$ 已知后我们观测到的数据应该是什么样子
   - 一大输出：后验$P(\theta|X)$，指最终的参数分布

   $$
   P(\theta|X) = \frac{P(X|\theta) \times P(\theta)}{P(X)}
   $$

   

   Bayesian学派的估计基础是Bayes's Theorem

   同样是抛硬币的例子，如果一个硬币抛5次得到5次正面，如果先验认为这个硬币大概率是均匀的，那么硬币朝上的概率$P = P(\theta|X)$是一个Distribution，最大值会介于0.5~1之间，而非武断的$\theta = 1$



---



**两点值得注意的地方**

- 随着数据量的增加，参数分布会向数据靠拢，**先验**的影响力会越来越小（先验是我们做的一个假设，数据量越多我们肯定会倾向跟随数据量的分布而逐渐降低先验的权重，毕竟如果一个分布是二分布，你假设它为某种奇怪的分布D，刚开始他可能会遵循奇怪的分布D，但是随着数据量的增加，它最终慢慢会变为一个二分布）
- 如果先验是**Uniform Distribution**，那么Frequentist和Bayesian相同，因为均匀分布实际上没有对事物做出任何预判



---



### MLE - Frequentist

最大似然分布，属频率学派

假设数据$x_1,x_2,...,x_n$是i.i.d.[^1]的一组**抽样**，随机变量$X = (x_1,x_2,...,x_n)$，那么MLE对$\theta$的Estimation如下
$$
\begin{equation}
\begin{aligned}
{\hat{\theta}_{MLE}} &= \arg\max P(X;\theta) \\ &= \arg\max P(x_1;\theta)P(x_2;\theta)···P(x_n;\theta) \\ &= \arg\max \log\prod_{i=1}^nP(x_i;\theta) \\ &=\arg\max\sum_{i=1}^n\log P(x_i;\theta) \\ &= \arg\min - \sum_{i=1}^n\log P(x_i;\theta)
\end{aligned}
\end{equation}
$$

1. 我们一般对似然函数进行$\log$运算，为了计算方便
2. 写出**对数似然函数**方程，求导找极值点$\theta$，使得**似然函数最大**
3. 最后一个方程为**NLL - 负对数似然**

**例1 : 抛硬币**

因为抛硬币可以表示为参数$\theta$的Bernoulli分布，所以
$$
P(x_i;\theta) = \begin{equation}\left\{\begin{aligned}\theta & , & x_i=1, \\1-\theta & , & x_i=0.\end{aligned}\right.\end{equation} = \theta^{x_i}(1-\theta)^{1-x_i}
$$
其中$x_i=1$表示第 $i$ 次抛出正面，根据已有公式
$$
{\hat{\theta}_{MLE}} = \arg\max\sum_{i=1}^n \log P(x_i;\theta) \\ = \arg\max\sum_{i=1}^n\log \theta^{x_i}(1-\theta)^{1-x_i}
$$
对其求导等于0，注意这里的求导对象是 $\theta$

得到$\hat{\theta} = \frac{\sum_{i=1}^nx_i}{n}$，也就是说该参数的最大似然情况是正面的次数除去总共的抛掷次数



**MLE的几个特性**

1. 估计量的无偏性 $E(\hat{\theta}) = \theta$
2. 估计量的有效性 $Var(\hat{\theta_1}) \leq Var(\hat{\theta_2})$ 也就是说，较好的参数估计两会更密集的分布在真实参数的附近，方差更小
3. 估计量的相合性 $\lim_{n\to\infty}P\{||\theta-\hat{\theta}|| < \epsilon\} = 1, \forall \epsilon$

上述三性，必须满足**相合性**



### MAP - Bayesian

最大后验估计，属贝叶斯学派

假设数据$x_1,x_2,...,x_n$是i.i.d.的一组**抽样**，随机变量$X = (x_1,x_2,...,x_n)$，那么MAP对$\theta$的Estimation如下
$$
\begin{equation}
\begin{aligned}
\hat{\theta}_{MAP} &= \arg\max P(\theta|X) \\ &=\arg\min - \log P(\theta|X) \\ &= \arg\min - \log\frac{P(X|\theta)P(\theta)}{P(X)} \\ &= \arg\min - \log P(X|\theta) - \log P(\theta) + \log P(X) \\ &= \arg\min - \log P(X|\theta) - \log P(\theta)
\end{aligned}
\end{equation}
$$

1. 值得注意的是，我们求的是和参数 $\theta$ 的关系，所以$\log P(X)$可以丢掉，其与$\theta$无关
2. $\log P(X|\theta)$是一个似然，是假设 $\theta$ 已知后我们观测到的数据应该是什么样子，其实也就是MLE中的$-\sum_{i=1}^n\log P(x_i;\theta)$，即NLL。
3. 也就是说，MAP的前半部分就是似然函数，后半部分是参数的先验分布，所以MLE和MAP相差的只是一个**先验项$\log P(\theta)$**
4. 假定先验项是一个Gaussian Distribution，即$P(\theta) = constant \times e^{-\frac{\theta^2}{2\sigma^2}}$，那么$-\log P(\theta) = constant + \frac{\theta^2}{2\sigma^2}$
5. 我们可以发现一点，**在MAP是用一个高斯分布的先验等价于在MLE中采用L2的Regularization**
6. 所以，**MAP可以看作是Regularization的MLE**
7. 最后的结果就是对上述式子求导取极值点，类似MLE



#### BE - Bayesian

贝叶斯估计，属贝叶斯学派

前面的部分同MAP
$$
P(\theta|X) = \frac{P(X|\theta) P(\theta)}{P(X)}
$$
因为这里的变量是$\theta$，X相当于一个常数，所以$P(X)$也相当于一个常数，我们可以用$\alpha$代替
$$
P(\theta|X) = \alpha P(X|\theta)P(\theta) = \alpha \prod_{i=1}^NP(x_i|\theta)P(\theta)
$$
在这里，和MAP有所不同

1. MAP认为，$\theta$是一个**确定但未知的参数**，所以MAP认为该式是$\theta$的一般函数，所以将估计问题转换为优化问题，求$\theta$使该式最大（用导数优化求最值）

2. BE认为，$\theta$是一个**未知的变量**，所以BE认为该式是$P(\theta|X)$的**后验概率密度函数**，我们要求的是参数$\theta$的估计值$\hat{\theta}$，要满足最小化下面的期望损失函数
   $$
   \arg\min_{\hat{\theta}}\int L(\hat{\theta},\theta)P(\theta|X)d\theta
   $$
   其中$\hat{\theta}$是估计值，$L(\hat{\theta},\theta)$是损失函数，一般可以选择二阶损失函数
   $$
   L(\hat{\theta},\theta) = (\hat{\theta} - \theta)^2
   $$
   我们将损失函数的表达式代入其中，可以得到
   $$
   \int L(\hat{\theta},\theta)P(\theta|X)d\theta \\ = \hat{\theta}^2\int P(\theta|X)d\theta - 2\hat{\theta}\int \theta P(\theta |X)d\theta + \int \theta^2P(\theta|X)d\theta
   $$
   对$\hat{\theta}$求导并令其等于0，得
   $$
   \hat{\theta}^* = \frac{\int \theta P(\theta|X)d\theta}{\int P(\theta|X) d\theta}
   $$
   考虑到分母显然为1（因为一个概率密度函数在整个区间上求积分，概率之和肯定为1）

   所以$\theta$得最终估计值为：即**参数$\theta$得后验概率期望**
   $$
   \hat{\theta}^* = \int \theta P(\theta|X)d\theta
   $$

## Summary MLE & MAP & BE

1. 在MLE中，$\theta$应该是**已经确定但是未知的**，所以MLE求得的结果是参数$\theta$的**一个点**。

2. 在MAP中，$\theta$同样是**已经确定但是未知的**，但是与MLE不同的是，MAP考虑了先验知识，也就是参数$\theta$根据我们的经验，满足一个概率分布，MLE求得的结果同样是参数$\theta$的**一个点**。
3. 在BE中，$\theta$压根就是**未知的变量**，我们利用贝叶斯定理得到了参数$\theta$的**后验概率分布**，而不是MLE或MAP那样得到一个点，只不过我们最终取**后验概率期望**为我们最终的估计值。

[^1]: Independent And Identical Distribution - 独立同分布

