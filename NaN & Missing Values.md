# NaN & Missing Values
## How to check null or missing values in a Pandas DataFrame
核心代码：`df.isnull().any().describe()`

---

```python
import pandas as pd
import numpy as np

train = pd.read_csv("train.csv")

X_train = train.drop(labels = ["label"], axis = 1) # 将最后一列删掉
Y_train = train["label"] # 最后一列作为Y_train

del train # 节省空间

# 检测Null & Missing Values
X_train().isnull().any().describe()
```

假设读入的矩阵为，生成该矩阵的代码为

```python
df = pd.DataFrame(np.random.randn(3,3))
df.iloc[1,2] = np.nan
df.iloc[2,0] = np.nan
df.iloc[2,2] = np.nan
```

| 1.232227      | **-0.872299** | **1.02378** |
| ------------- | ------------- | ----------- |
| **-0.309492** | **1.464088**  | **NaN**     |
| **NaN**       | **0.105063**  | **NaN**     |

2. 这里关键的Code Snippets是`X_train.isnull().any().describe()`

   - `.isnull()`会返回一个`shape == X_train.shape`的矩阵，矩阵中的元素分别是True和False，代表是否是null，输出矩阵为

     | True      | **True** | **True**  |
     | --------- | -------- | --------- |
     | **True**  | **True** | **False** |
     | **False** | **True** | **False** |

   - `any()`函数用于判断给定的可迭代参数**iterable**是否全部为False，如果含有False，则返回True，代表有缺失值。如果没有缺失值，则返回False

     ```
     0     True
     1     False
     2     True
     dtype: bool
     ```

   - `describe()`输出描述性统计信息

     ```python
     count        3 # 非空值数，这里代表3列
     unique       2 # 唯一值数，这里为2代表有False和True，所以肯定有NaN
     top       True # 频率最高，说明True多，NaN所在的列也较多
     freq         2 # 频率
     dtype: object
     ```

如何检测是否有空或者缺失值，也可以参考这篇文章[^1]

[^1]: How to check if any value is NaN in a Pandas DataFrame https://stackoverflow.com/questions/29530232/how-to-check-if-any-value-is-nan-in-a-pandas-dataframe