## 1. Definition
- 定义：One-hot编码，又称为一位有效编码。Data Preprocessing的常见方式
- 方式：采用N位状态寄存器来对N个状态进行编码，**任意时刻只有一位有效**
- 例如，我们有性别特征["男", “女"]，同时有国家特征["中国","美国","法国"]
	1. 用一个二维特征向量来表示性别，编码用[0,1]表示男，[1,0]表示女。
	2. 用一个三维特征向量来表示国家，三个国家分别对应[0,0,1],[0,1,0],[1,0,0]
	3. 则如果一个样本的特征为美国女性，则可以表示为[1,0,0,1,0]，前两位代表性别，后三位代表国家

--- 

## 2. Why do we need one-hot encoding ?
1. 在「Regression, Classification, Clustering」问题中，计算特征之间的距离 | 相似度较为频繁，**计算相似性大多在欧式空间**[^1]
2. One-hot编码可以**将离散取值扩展到**欧式空间

---

## 3. Implementation
### 3.1 Keras
#### 3.1.1 One-hot encoding of words
- 用Keras实现单词级的one-hot编码 [^2]

```python
# vectorize : 以单词为单位进行one-hot编码
import numpy as np
from keras.preprocessing.text import Tokenizer

samples = ['The cat sat on the mat.', 'The dog ate my homework.']

tokenizer = Tokenizer(num_words = 1000) # 分词器tokenizer，只考虑频率最常见的1000个单词
tokenizer.fit_on_texts(samples) # 构建单词索引

sequences = tokenizer.texts_to_sequences(samples) # 将单词转换为其对应的[整数索引]
# 实现vectorize

one_hot_results = tokenizer.texts_to_matrix(samples, mode = 'binary')

word_index = tokenizer.word_index # 找回单词索引
print(word_index)
print('Found %s unique tokens.' % len(word_index))

--------------------------------------------------------
Output:
{'the': 1, 'cat': 2, 'sat': 3, 'on': 4, 'mat': 5, 'dog': 6, 'ate': 7, 'my': 8, 'homework': 9}
Found 9 unique tokens.
```
- 此时one_hot_results.shape为[2,1000]，2代表我们考虑了两个句子，1000代表我们只考虑1000高频单词。`one_hot_results[0][i]`代表在第一个句子中，单词索引为`i`的词是否出现过，出现为1否则为0.
## References
[^1]:机器学习：数据预处理之独热编码（One-Hot）https://zhuanlan.zhihu.com/p/39012149
[^2]:Deep Learning with Python Chapter 6