# Bayes's Theorem

## Definition

**贝叶斯定理：**对于事件A和事件B，有
$$
P(B|A)·P(A) = P(A|B)·P(B)
$$
因此只要$P(B) \neq 0$，则有
$$
P(A|B) = P(B|A)·\frac{P(A)}{P(B)}
$$

---

1. **推导过程**：根据[[Conditional Probability]]可以得到$P(A \cap B) = P(A|B)·P(B)$，且$P(A \cap B) = P(B|A)·P(A)$。联立两个公式消掉$P(A \cap B)$即可得到$P(B|A)·P(A) = P(A|B)·P(B)$

2. **二元划分下的Bayes's Theorem** - 我们可以将B划分为两个互不相交的事件，即$A^c \cap B$和$A^ \cap B$，则$P(B)$满足以下关系

$$
P(B) = P(B|A)P(A) + P(B|A^c)P(A^c)
$$

所以我们可以推导如下公式
$$
P(A|B) = P(B|A)·\frac{P(A)}{P(B)} = \frac{P(B|A)·P(A)}{P(B|A)P(A)+P(B|A^c)P(A^c)}
$$

3. 全划分下的**Bayes's Theorem** - 
   1. 样本空间的一个划分 - 样本空间$\Omega$的就是满足下列条件的可数个集合$\{A_1,A_2,···\}$
      - 如果$i \neq j$，则$A_i \cap A_j = \emptyset$
      - $\cup_iA_i = S$
   2. 假设$\{A_1,A_2,···,A_n\}$是样本空间S的一个划分，则$P(B) = \sum_{i=1}^n P(B|A_i)·P(A_i)$
   3. 从而Bayes's Theorem变为

   $$
   P(A|B) = \frac{P(B|A)·P(A)}{ \sum_{i=1}^n P(B|A_i)·P(A_i)}
   $$

   