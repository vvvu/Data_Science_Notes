# Random Variable
## Definition
**Definition 1.0 - Basic Concept**
1. 样本空间$\Omega$（随机现象实验结果的集合）
2. 事件，$\Omega$样本空间的子集，记为$2^\Omega$
3. 事件域$F$，事件$2^\Omega$的子集，称之为事件域
4. 概率$P$，在$F$上定义概率

**Definition 1.1 - 1D Borel Set**
>由一切形为[a,b)的有界左闭右开区间构成的集类所产生的事件域，称为1D-Borel Set，简记为B

1. 一维博类尔点集
2. 实际上，所有能想到的点集基本都属于1D-Borel set
3. 1D-Borel set是事件域，所以可以在上面定义概率

---

**Definition 1.2 - Random Variable**
> 设$\xi$是定义在概率空间$(\Omega,F,P)$上的实函数，如果对于任意一维博雷尔点集B有$$\{\omega:\xi(\omega)\in B\}\in F$$则称$\xi(\omega)$为随机变量，简记为$\xi$

1. 所有能映射到博类尔点集的事件都属于概率空间的事件域$F$，也就是说**我们对$\xi(\omega)$做一些操作后生成的集合仍然在事件域中**

**Definition 1.3 - CDF**
定义参考[[Cumulative Distribution Function]]
> 称$F(x) = P\{\xi(\omega) < x\}$为随机变量$\xi(\omega)$的累积分布函数，简记为$\xi\sim F(x)$

1. 与概率的关系：$P(a\leq\xi\leq b) = P(\xi < b) - p(\xi < a) = F(b) - F(a)$

**Definition 1.4 - PDF**
定义参考[[Probability Density Function]]
> 对于随机变量$\xi$的累积分布函数$F(x)$，如果存在非负可积函数$f(x)$，使对于任意实数有$$F(x)=\int_{-\infty}^xf(t)dt$$，称$\xi$为连续型随机函数，$f(x)$称为$\xi$的概率密度函数（PDF）。
> 对于概率$P$有$$P\{a\leq\xi\leq b\} = F(b)-F(a)=\int_a^bp(x)dx$$
1. 根据概率的定义可知，$\int_{-\infty}^{\infty}f(t)dt=1$

**PDF & CDF => 概率密度函数的积分就是累积分布函数，即$\int PDF = CDF$**

---
## Numerical Details of Random Variable
### Mean
[[Mean]] 均值，期望，反应Random Variable「集中」在哪个值上

**Definition 2.1**
>$\xi$是**离散型随机变量**，其分布律如下：$$P\{\xi=x_k\}=p_k,k=1,2,···$$则随机变量 $\xi$ 期望为$$\sum_{i=1}^\infty x_kp_k$$简记为$E(\xi)$

**Definition 2.2**
>$\xi$是**连续型随机变量**，其概率密度函数如下：$$p(x)=f(x)$$则如下值为随机变量 $\xi$ 的期望：$$\int_{-\infty}^\infty xf(x)dx$$	简记为$E(\xi)$

### Variance
[[Variance]] 方差，记录Random Variable的偏离程度，记作$Var$或$D(\xi)$，这里的$D$是Deviation（偏离）的简写

**Definition 2.3**

>设 $\xi$ 是一个Random Variable，若$E(\xi)$存在，且$E\{[\xi-E(\xi)]^2\}$也存在，则$Var(\xi) = E\{[\xi-E(\xi)]^2\}$

标准差为方差开根号，记作 $\sigma(\xi)$

### Covariance
[[Covariance]] 协方差，描述两个Random Variable的相互关系

**Definition 2.4**

>设有两个随机变量$\xi_1,\xi_2$$$Conv(\xi_1,\xi_2)=E\{(\xi_1-E(\xi_1))(\xi_2-E(\xi_2))   \}$$称为随机变量$\xi_1,\xi_2$的Covariance$$\rho\xi_1\xi_2=\frac{Conv(\xi_1,\xi_2)}{\sqrt{Var(\xi_1)} \sqrt{Var(\xi_2)}}$$成为随机变量$\xi_1,\xi_2$的相关系数

1. Variance是Covariance的一种特殊情况$$Conv(\xi_1,\xi_1)=Var(\xi_1)$$
2. $|\rho\xi_1\xi_2|$接近于1，表示$\xi_1,\xi_2$线性关系越紧密，$|\rho\xi_1\xi_2|$接近于0，表示$\xi_1,\xi_2$线性关系越不紧密。所以$|\rho\xi_1\xi_2|$的大小表示了$\xi_1,\xi_2$的**线性相关性**
3. 一般的，当$|\rho\xi_1\xi_2|=0$时，我们称$\xi_1,\xi_2$不相关（即没有线性关系）

**Definition 2.5**
协方差矩阵：即n维情况下的协方差情况
>对于$n$维的随机变量$(\xi_1,\xi_2,...,\xi_n)$$$\begin{pmatrix}
c11 & c12 & ... & c1n \\ c21 & c22 & ... & c2n \\ ... & ... & ... & ... \\ cn1 & cn2 & ... & cnn
\end{pmatrix}$$其中$$c_{ij}=Conv(\xi_i,\xi_j)$$

1. 对称矩阵，因为$Conv(\xi_i,\xi_j) = Conv(\xi_j,\xi_i)$
2. 对角线元素$c_ii$是$\xi_i$的方差$Var(\xi)$

## Moment
**Definition 2.6**
[[Moment]] 矩(Moment)，矩是期望、方差和协方差的一个自然的推广。它统一了期望、方差和协方差的定义。

> 设有$\xi_1,\xi_2$两个Random Variable
> 如果$E(\xi_1^k),k=1,2,···$存在，称为$\xi_1$的**k阶原点矩**
> 如果$E((\xi_1-E(\xi_1))^k,k=2,3,···$，称为$\xi_1$的**k阶中心矩**
> 如果$E((\xi_1-E(\xi_1))^k_1E((\xi_2-E(\xi_2))^k_2$存在，称它为$\xi_1,\xi_2$的**$k_1+k_2$阶混合中心矩阵**

由上述定义可知
1. 期望是一阶的原点矩
2. 方差是二阶中心矩
3. 协方差是二阶混合中心矩