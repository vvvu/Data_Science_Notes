# PCA
**我们先定义符号表示规则：N表示数据数量，$n_d$表示Input Features的维数，$n_e$表示Encode后Latent Space的维数**

PCA的主要想法是，根据Input输入的$n_d$个Features，构建$n_e$个新的独立特征，并希望新的独立特征可以尽可能接近Input Space。换句话说，**PCA正在寻找Input Space的最佳线性子空间，该子空间由$n_e$个新的正交独立特征构成，子空间评判标准是让Input Space在该子空间上的投影误差尽可能的小**

![1*ayo0n2zq_gy7VERYmp4lrA@2x](https://miro.medium.com/max/2000/1*ayo0n2zq_gy7VERYmp4lrA@2x.png)