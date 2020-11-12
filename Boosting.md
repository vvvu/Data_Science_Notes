# Boosting
## Abstract
1. 适用场景：适合较弱的分类器 -> Improve Weak Classifiers
2. Weak Classifiers - 弱分类器指一个Classifiers的效果只比随机分类好一点点，如二分类中分类效果可能只比50%多一点，四分类中分类效果可能只比25%多一点
3. 后一个Model关注前一个Model表现不佳的地方，且因为此所以是**Learning Sequentially** 
4. **Boosting Model's** key is learning from the **previous mistakes.**
## Process
![[Screen Shot 2020-11-09 at 16.02.53.png]]
**Boosting**算法的一个最直观思想为：后面的 Model 需要关注前面 Model 表现不佳的地方
1. 在Entire Dataset上训练 Model $f_1(x)$
2. 对 $f_1(x)$ 表现较差的数据提高权重，在Reweighted Entire Dataset上训练 Model $f_2(x)$
3. 对 $f_2(x)$ 表现较差的数据提高权重，在Reweighted Entire Dataset上训练 Model $f_3(x)$
4. ...
## Algorithms & Models
- [[AdaBoost]]
- [[Gradient Boosting Decision Tree]]