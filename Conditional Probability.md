# Condition Probablity
## Definition
- 设B是满足条件$P(B) > 0$的事件，那么在B发生的条件下A发生的概率为$P(A|B) = P(A\cap B)/P(B) = \frac{P(A\cap B)}{P(B)}$
	1. 新条件：要求$P(B)>0$是因为我们讨论的是B发生的情况下A发生的概率，如果B没有发生讨论当前条件概率是**没有意义的**。此时若$P(B) = 0$，则$P(A \cap B)/P(B)$会得到一个$\frac{0}{0}$
	2. 样本空间缩减：如果我们将B换做整体样本空间$\Omega$，则$P(A|\Omega) = P(A\cap \Omega)/P(\Omega) = P(A)$
	3. Venn图：图中两个圆圈相交的地方就是A、B两个事件都发生的概率。此时样本空间为$\Omega$，题目假设为求解在B发生的条件下A发生的概率，则此时样本空间缩减为$B$，所以我们只需要$P(A \cap B)/P(B)$即可

- 公式推导
	
	- 统计A和B的发生情况表格。其中$A^c,B^c$分别代表A不发生，B不发生
	
	- |       | $A$          | $A^c$          |
	  | ----- | ------------ | -------------- |
	  | $B$   | $A \cap B$   | $A^c \cap B$   |
	  | $B^c$ | $A \cap B^c$ | $A^c \cap B^c$ |
	
	  考虑在B发生的条件下A发生的概率。B发生的条件 => 表格第一行，A发生的概率 => $A \cap B$。则条件概率公式为$P(A|B) = \frac{P(A \cap B)}{P(A \cap B) + P(A^c \cap B)} = \frac{P(A \cap B)}{P(B)}$


