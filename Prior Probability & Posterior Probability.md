# Prior Probability & Posterior Probability
## Definition
- Prior Probability - 根据**以往的经验和分析**得到的概率
- Posterior Probability - 在**得到结果后**重新修正的概率，Posterior Probability的计算要以Prior Probability为基础，通过[[Bayes's Theorem]]用Prior Probability和[[Likelihood Function]]计算

## A Example To Compare Prior & Posterior Probability
假设我们已北方人口占全部人口的60%，南方人口占全部人口的40%。，我们用$X$表示随机变量
$$
P(X=North) = 0.6, P(x = South) = 0.4
$$
这个概率是**统计得到的，或者是依据自身经验得到的**，我们称之为**Prior Probability**，一般来讲，Prior Probability就是我们通常所说的概率
此外，北方人口有80%是男性，20%是女性，南方人口中20%是男性，80%是女性，用$Y$表示性别随机变量，我们可以得到以下条件分布
$$
P(Y=M|X=North) = 0.8,P(Y=F|X=North) = 0.2
$$
$$
P(Y=M|X=South) = 0.2,P(Y=F|X=South) = 0.8
$$
如果我们现在想知道**已知一个人为男性，那么他是北方人的概率有多大**
根据Bayes's Theorem可以得到
$$
P(X=North|Y=M) = P(Y=M|X=North)\frac{P(X=North)}{P(Y=M)}
$$
$$
=\frac{P(Y=M|X=North)P(X=North)}{P(Y=M|X=North)P(X=North)+P(Y=M|X=South)P(X=South)}
$$
这个概率$P(X=North|Y=M)$称之为**Posterior Probability**，即它是在观察到事件$Y$发生后得到的

## Summary
1. Prior Probability是以**全事件**为背景下，A事件发生的概率。$P(A|\Omega)$，这个概率一般是**统计**得到的，所以称之为先验概率，即没有实验前我们已知的概率。
2. Posterior Probability是以**事件B**为背景下，A事件发生的概率。$P(A|B)$。事件B一般是实验，此时的事件背景从全事件$\Omega$变成了B，所以对A的概率会有一定的影响，**那么现在需要对A的概率进行一个修正**，从$P(A|\Omega)$变为$P(A|B)$。所以称为后验概率，也就是试验（事件B）发生后的概率