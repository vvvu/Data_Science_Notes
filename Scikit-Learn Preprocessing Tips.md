# Scikit-Learn Preprocessing Tips
## 1. Apply Different Preprocessing to Different Columns
通过`make_column_transformer`函数来对不同的Column使用不同的数据预处理方式[^1]
相关的Preprocessing API可以在官方文档[^2]找到不同的数据预处理方式
```python
# Load Python Package
import pandas as pd
from sklearn.preprocessing import OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.compose import make_column_transformer

# Load data (loading Titanic dataset)
data  = pd.read_csv('./train.csv') # Or other dataset
# Make Transformer
preprocessing = make_column_transformer(
    (OneHotEncoder(), ['Pclass','Sex']),
    (SimpleImputer(), ['Age']),
    remainder='passthrough')

# Fit-Transform data with transformer
new_data = preprocessing.fit_transform(data)
```

[^1]: 9 Scikit-Learn Tips for Data Scientist https://medium.com/@simonprdhm/9-scikit-learn-tips-for-data-scientist-2a84ffb385ba
[^2]: sklearn.preprocessing: Preprocessing and Normalization https://scikit-learn.org/stable/modules/classes.html#module-sklearn.preprocessing