# Law of Large Numbers
大数定理：在试验不变的条件下，重复试验多次，**随机事件的频率近似于它的概率**。偶然中包含着某种必然。
例如：我们可以可以认为，如果我们多次扔硬币，最后硬币出现结果正反的频率基本上是接近于0.5,0.5的

用数学语言表示可以表示为一种极限$$\lim_{n\to\infty}P\{|\frac{\xi}{n}-p| < \epsilon\} = 1$$，通俗地讲，对于随机变量$\xi$，随着试验次数的增加，$|\frac{\xi}{n}-p|$很有可能接近于概率$p$
也就是说，$\frac{\xi}{n}$很大可能接近于$p$（前者为频率，后者为概率）

**大数定理的作用为：让我们可以用有限的数据来逼近一个未知的东西**