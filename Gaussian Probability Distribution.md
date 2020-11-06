# Gaussian Probability Distribution
## Gaussian Distribution
1. 若随机变量$X$服从一个数学期望为$\mu$，方差为$\sigma^2$的高斯分布。记作$N(\mu,\sigma^2)$
2. 一维高斯分布的概率密度函数$$f(x) = \frac{1}{\sigma\sqrt{2\pi}}exp(-\frac{(x-\mu)^2}{2\sigma^2})$$
	当$\mu = 0, \sigma = 1$时，记作**标准高斯分布** $$f(x) = \frac{1}{\sqrt{2\pi}}exp(-\frac{x^2}{2})$$
	
---

##  Multivariate (Bivariate) Gaussian Distribution
1. 概率分布函数为$$f_{X}(x_1,x_2,...,x_k) = \frac{1}{\sqrt{(2\pi)^k |\Sigma|}}exp(-\frac{1}{2}(\bf{x-\mu}^T)\Sigma^{-1}(\bf{x-\mu}))$$
	这里的$|\Sigma|$表示**协方差矩阵**的行列式