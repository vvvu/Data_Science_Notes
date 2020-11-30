## Variational Auto-Encoder

**For a random variable z, we will denote p(z) the distribution**

1. VAE是一种自动编码器，在训练过程中其编码分布是规则的。以确保其Latent Space具有良好的属性，从而使我们能够生成一些新的数据。
2. Variational来自统计中的Regularisation和[[Variational Inference]]之间的密切关系

### 1. Dimensionality Reduction, PCA and Auto-encoders

谈及降维，我们可以想到两种方法 - [[Principal Components Analysis (PCA)]] 和 Auto-encoders

#### 1.1 What is dimensionality reduction ?

> **Dimensionality Reduction is the process of reducing the number of features that describe some data**

降维的本质就是减少特征，可以通过

1. Selection - 选择保留一些现有Features
2. Extraction - 提取 - 基于旧特征来创建 => 维度降低的新特征

在AE中，**Encoder**通过 Selection 或 Extraction 方法进行压缩数据（从Initial Space到**Encoded Space**，也称之为**Latent Space**）。**Decoder**将会解压缩Latent Space中的数据

- 这个压缩一定是有损的，意味着一部分信息在Encode过程中丢失，并且在Decode时无法恢复

![[Attachment/Pasted image 20201130150906.png]]

- Dimensionality Reduction的主要目的是找到最佳的Encoder和Decoder，即可以在Encode保有最有效信息，Docder时解码最有效信息的。显然，我们希望Reconstruction Error的值越小越好。

- 综上，Dimensionality Reduction问题可以定义如下
  $$
  (e^*,d^*) = \arg\min_{(e,d)\in E \times D} \epsilon(x,d(e(x)))
  $$
  

  其中$\epsilon(x,d(e(x))$定义了输入输出的Reconstruction Error，如果我们定义$x^* = d(e(x))$，则Reconstruction Error可以定义为$\epsilon(x,x^*)$

#### 1.2 The Introduction of Principal Components Analysis (PCA)

**我们先定义符号表示规则：N表示数据数量，$n_d$表示Input Features的维数，$n_e$表示Encode后Latent Space的维数**

PCA的主要想法是，根据Input输入的$n_d$个Features，构建$n_e$个新的独立特征，并希望新的独立特征可以尽可能接近Input Space。换句话说，**PCA正在寻找Input Space的最佳线性子空间，该子空间由$n_e$个新的正交独立特征构成，子空间评判标准是让Input Space在该子空间上的投影误差尽可能的小**

![[Attachment/Pasted image 20201130150914.png]]

#### 1.3 Autoencoders

![[Attachment/Pasted image 20201130150920.png]]

AE的总体思想很简单，这里**不再赘述**

假设我们的Encoder和Decoder只有一层Layer，且没有经过非线性操作。也就是说，我们假设Encoder和Decoder是简单的线性变换=>可以表示为矩阵。

- 在这种情况下，我们其实和PCA的思路非常相似，目标就是寻找一个最佳的线性子空间来投影数据，并要求投影损失尽可能的少。

- 此时，PCA中的Encode Matrix和Decode Matrix自然是我们目前优化的方向之一，但并不是唯一的解决方案。

  >**several basis can be chosen to describe the same optimal subspace**

- 事实上，我们可以选择很多个Basis来描述同一个最佳子空间。所以Serveal Encoder/Decoder Pairs可以给出最优的Reconstruction Error，而并非只有一个。
- 同时和PCA对比，我们经过Encode得到的Latent Space的Features并不需要是独立的（NN中没有正交性约束）

Latent Space的维数和AE的深度需要不断的调整，原因如下

1. Latent Space中缺乏Interpretable and Exploitable Structures => **Lack of regularity**
2. 我们希望在降维的同时，将Main Structure of Input Data保留在降维后的表示中 => When reducing dimensionality, we want to keep the main structure there exists among the data.

### 2. Variational Autoencoders

VAE的提出肯定是为了解决AE在一些问题上的局限性

#### 2.1 Limitations of Autoencoders For Content Generation

Autoencoders和Content Generation之间有什么联系呢？

1. 我们一旦得到训练后的Autoencoders，我们实际上就得到了训练好的Encoder和Decoder

2. 如果Latent Space足够有规律[ The regularity of the latent space ] ，每个维度的意义比较明朗，我们就可以Latent Space中随机取一个点，进行Decode就可以得到一个新的内容

3. 此时，Decoder的作用就会像一个Content Generator

   > We can generate new data by decoding points that are randomly sampled from the latent space. The quality and relevance of generated data depend on [ the regularity of the latent space. ]

![[Attachment/Pasted image 20201130150930.png]]

我们发现Auto-encoder的Latent Space的规律性[ The regulartiy of the latent space ]是一个难点。它取决于[ Input Distribution, The dimension of The Latent Space, The Architecture of The Encoder ]

> Irregular latent space prevent us from using autoencoder for new content generation.

- 实际上，我们可以让Encoder将每个Input编码成为一个数轴上的点，比如如果输入是（39岁，男性，医生）编码成0，（25岁，女性，产品经理）编码成1，（55岁，男性，退休）编码成2。这样我们将3D的Input降维成了1D的Latent Space，而且可以No Reconstruction Error在Decoder还原出来。
  - 实际上，我们用这种将input和latent space一一对应的方式，可以将所有输入都转换为 1D Latent Space。「但这种情况显然是一种严重的**Overfitting**，也是**Auto-encoder的局限性**。」
  - 这样意味着Latent Space的一些Point经过Decoder会给出无意义的内容，例如如果我们提取Latent Vector，值为4，因为前面只出现过0，1，2，我们把4交付给Decoder他其实也不知道要生成什么东西，**从而会给出一些无意义的内容**
  - 我们的理想情况是，我们可以进行适当的降维，例如降维到2D Latent Space，其中每个元素都有较为明确且规律的含义。比如[x,y]，可能对于AE来说，x就代表年龄和性别，y代表职业，这样我们将x的值调大一点，我们可能就会得到（66岁，男性，医生），**显然这是一个全新的值**，在这种情况下，因为Latent Space的规律性，我们才可以利用Decoder来**生成一些我们倾向于生成的东西**

- 所以，我们为了防止Auto-encoder的**Overfitting**情况（见上文），我们可以需要对其进行正则化**Regularization**

#### 2.2 Definition of Variational Auto-encoders

根据前文我们知道，为了让AE成为一个Content Generator，我们需要确保Latent Space要有一定的规律性。我们需要对其进行Regularization。

![[Attachment/Pasted image 20201130150943.png]]

我们的做法是，**并非将Input编码为一个Single Point，而是Encode为在Latent space上的一个Distribtuion** => 这也是AE和VAE的不同，AE是确定性的，VAE是概率性的。

> **instead of encoding an input as a single point, we encode it as a distribution over the latent space**

我们将Training过程修改如下

> - First, the input is encoded as distribution over the latent space => 将Input编码为在Latent Space上的Distribution
> - Second, a point from the latent space is sampled from that distribution => 从该Distribution中Sample A Point
> - Third, the sampled point is decoded and the reconstruction error can be computed => Decode并且计算Reconstruction Error
> - Finally, the reconstruction error is backpropagated through the network => 反向传播

在实践中，编码后的分布**Encoded Distributions**被选为**Gaussian Distribution**，这样Encoder实际上传递的是描述这个Gaussian Distribtuion的Mean和Variance，如图所示

![[Attachment/Pasted image 20201130151001.png]]

- 为何Encode结果我们选用Distribution而非Single Point

  > he reason why an input is encoded as a distribution with some variance instead of a single point is that it makes possible to express very naturally the latent space regularisation: the distributions returned by the encoder are enforced to be close to a standard normal distribution. We will see in the next subsection that we ensure this way both a local and global regularisation of the latent space (local because of the variance control and global because of the mean control).

  1. 它可以比较自然地表达Latent Space的正则化（Regularization）
  2. Encode编码得到的Distribtuion会被强制要求接近Standard Normal/Gaussian Distribution
  3. **我们以这种方式确保了Latent Space的局部正则化和全局正则化 => 局部正则化是由Variance控制，全局正则化是由Mean控制）**

- VAE的损失函数如下，由两部分构成

  > In variational autoencoders, the loss function is composed of a reconstruction term (that makes the encoding-decoding scheme efficient) and a regularisation term (that makes the latent space regular).

  $$
  loss = ||x-\hat{x}||^2+KL[N(\mu_,\sigma_x) ||N(0,1)] = ||x-d(z)||^2+KL[N(\mu_,\sigma_x)||N(0,1)]
  $$

  1. 第一项即Reconstruction Error
  2. 第二项倾向于让Encode返回的Distribution接近Standard Gaussian Distribution，从而让Latent Space正则化。两个分布之间的相似程度我们用KL散度来表示

#### 2.3 Intuitions about the regularisation

为了更好的体现Generative的特性，人们对于The Regularity of Latent Space的期望可以通过**Two main properties**来表示

![[Attachment/Pasted image 20201130151052.png]]

1. Continuity - Latent Space中两个距离接近的点不应该Generate完全不同的内容
2. Completeness - 对于一个选定的Distribution，我们在Latent Space中采样一个点，应该在Decode后给出有意义的内容

**我们在此继续论述为什么要强制Encode返回的分布要和Standard Gaussian Distribution接近**

1. 如果只是单纯的将Encoder的输出结果Encode为Distribution而非Single Point，是无法确保Continuity和Completeness的。**如果没有明确定义的(Well-defined)正则化项**

   为了让Reconstruction Error最小，在没有明确定义的正则化项时，Encoder将会表现的和Auto-encoder很类似。但是因为我们要求返回的是Distribution，所以会产生如下两种情况

   1. 返回具有微小Variance的分布（往往是点状分布/单点分布**Punctual Distributions**），点状分布其实和返回Single Point差不多
   2. 返回具有巨大Mean的分布，这导致在Latent Space中相距很远。

   上述两种情况都不满足Continuity和Completeness，**我们要求强制返回Distribution的方式其实并没有起到什么效果 => 所以我们需要正则化项**

2. 所以我们需要正则化相来让**返回Distribution**这个事情Work起来，我们的解决方案是让返回的Distribution强制接近Standard Normal Distribution
   1. Convariance Matrix => 接近于单位矩阵，防止出现单点分布**Punctual Distributions**
   2. Mean => 0，防止Encoded Distribtuion彼此距离太远

经过正则化后，我们强制返回Distribution这个事情Work了起来，可以得到下图这样的效果

![[Attachment/Pasted image 20201130151106.png]]

**然而，这样的 Model 构建方式和Reconstruction Error尽可能低是矛盾的**

1. Model本身为了最小化Reconstruction Error，本身就倾向于点状分布（即Low Variance，High Mean的Distribution）
2. 而我们的正则化项，显然违背了上述原则，为了得到Continuity和Completeness，我们会让Reconstruction Error变高

**所以我们需要对这对矛盾的双方进行trade-off，后文的Formal Derivation可以如何进行Trade-off**

---

最后进行一个简单的总结，**正则化实际上在我们的Latent Space中产生了一种Gradient**，如下图所示

![[Attachment/Pasted image 20201130151113.png]]

如何来解释这个Gradient，原文是这样解释的

> A point of the latent space that would be halfway between the means of two encoded distributions coming from different training data should be decoded in something that is somewhere between the data that gave the first distribution and the data that gave the second distribution as it may be sampled by the autoencoder in both cases.

我们可以这样举例子解释

- 如果我们有两个月亮的形状，一个是「满月」，一个是「新月（即看不见月球的情况）」。如果我们用传统的Auto-encoder中的decoder部分生成，我们其实并不知道根据latent space他输出的结果是什么。
- 但是在VAE中，我们经过正则化以后，整个Latent Space是Continuity，如上图所示，我们很有可能就会生成一个介于（新月）和（满月）之间的状态，也就是「半月」
- 而在AE中这是不可能的，因为两个月亮的形状在Latent Space中对应的位置大概率是不连续的，所以我们无法**有目的的输出我们想要的结果**，而在VAE中，两个月亮的位置是连续的，所以我们如果取中值就可以**有目的的得到我们想要的半月**

### 3. Mathematical Details of VAEs

总结一下前文内容

1. VAE将Input Encode为Distribution而非Single Point
2. 将Latent Space结构通过强制让其接近Standard Gaussian Distribution来正则化（从而让Distribution特性Work，上文已解释过）

#### 3.1 Probabilistic Framework & Assumptions

![[Attachment/Pasted image 20201130151119.png]]

我们定义$x$是我们Generate得到的变量，$z$是处于Latent Space的一个Latent Variable，且假定$x$是由未直接观察到的Latent Variable $z$（也就是Encoded Representation）生成的

生成过程可以概括为如下两个步骤

1. 从先验分布$p(z)$中Sample一个Latent representation $z$
2. 数据$x$从Conditional likelihood $p(x|z)$采样得到（这实际上就是一个条件概率，在已知$z$的情况下问$x$）

---

在当前这种Probabilistic Model下，我们可以重新定义Encoder和Decoder

> 显然因为传统的Auto-encoder是确定性的，而VAE则是一个概率编码器，描述的是Distribtuion而非Single Point

1. Probabilistic Encoder - $p(z|x)$**, that describes the distribution of the encoded variable given the decoded one**.
2. Probabilistic Decoder - $p(x|z)$**, that describes the distribution of the decoded variable given the encoded one**

我们可以用**Bayes's Theorem**让这几个概率发生联系
$$
p(z|x)=\frac{p(x|z)p(z)}{p(x)} = \frac{p(x|z)p(z)}{\int p(x|u)p(u)du}
$$
作者在这里做出了一系列**假设**，可以表达为如下关系式
$$
p(z) \equiv N(0,1) \\
p(x|z) \equiv N(f(z),cI), f\in F,c >0,I == Identity Matrix
$$

1. 假设$p(z)$是一个Standard Gaussian Distribution

2. $p(x|z)$也是一个Standard Gaussian Distribution。
   - Mean - 由变量$f(z)$确定，其中$f(z)$属于函数族F，目前我们并不指定$f(z)$具体是什么
   - CoVariance - 协方差矩阵由$cI$决定，$c$为正常数，$I$为单位矩阵（这里可以这样理解，如果是一个1D Standard Gaussian Distribution，其Variance为1，则在多维的情况下，使用$I$单位矩阵也可以让协方差矩阵的方差为1，这是由多维Gaussian Distribtuion的定义所决定的）
3. 所以**理论上，我们是可以计算$p(z|x)$**的，但是因为$p(x)$，也就是分母处的积分难以计算，所以我们需要使用**Variational Inference**近似技术

#### 3.2 Variational Inference Formulation

Variational Inference是一种**近似复杂Distribution**的技术，主要想法如下

1. 设定一个「参数化的分布族」（例如Gaussian族，参数是Mean和Variance）
2. 在该族中寻找Target Distribution的最佳**近似分布**（衡量方式可以考虑近似分布和目标分布之间的KL Divergence）
3. 不断进行Gradient Descent，对参数进行更新寻找最佳近似部分。

在这里，我们希望通过Gaussian Distribution来近似$p(z|x)$，它的两个参数Mean和CoVariance由两个函数$g,h$定义，分别属于不同的，可以确定的，参数化的函数族
$$
q_x(z) = N(g(x),h(x)),g\in G,h \in H
$$

> 在这里我们重复一下作者人为定义的两个假设
>
> 1. 假设$p(z)$是一个Standard Gaussian Distribution
>
> 2. $p(x|z)$也是一个Standard Gaussian Distribution

我们的目标现在是想找到和$p(z|x)$最接近的近似分布$q_x(z)$ => 实质上是找到$q_x(z)$这个Gaussian Distribution的两个参数 => 实质上是在函数族G，H中寻找最佳的两个函数$g,h$ => 过程如下
$$
\begin{equation}
\begin{aligned}
(g^*,h^*) &= \arg\min_{(g,h)\in G\times H}KL(q_x(z),p(z|x)) \\ &= \arg\min_{(g,h)\in G\times H}(E_{z\text{~}q_x}(logq_x(z))-E_{z\text{~}q_x}(log\frac{p(x|z)p(z)}{p(x)})) \\ &= \arg\min_{(g,h)\in G\times H}(E_{z\text{~}q_x}(logq_x(z))-E_{z\text{~}q_x}(logp(z))-E_{z\text{~}q_x}(logp(x|z))+E_{z\text{~}q_x}(logp(x))) \\ &= \arg\max_{(g,h)\in G\times H}(E_{z\text{~}q_x}(logp(x|z)-KL(q_x(z),p(z))) \\ &= \arg\max_{(g,h)\in G\times H}(E_{z\text{~}q_x}(-\frac{||x-f(z)||^2}{2c})-KL(q_x(z),p(z)))
\end{aligned}
\end{equation}
$$

> 对这个过程推导进行一定的解释
>
> 1. 第一行属于问题定义，即计算近似分布和目标分布的KL Divergence，并期望让参数最小
>
> 2. 第二行代入KL Divergence的公式，并将$p(z|x)$利用Bayes公式进行变换
>
> 3. 第三行对数公式变换一下
>
> 4. 第四行中
>
>    - 因为第三行的最后一项$E_{z\text{~}q_x}(log(p(x)))$其实是一个常数，所以我们在求解argmin时候可以忽略它（为什么是常数，因为$x$其实是给定的，我们虽然不知道$p(x)$是多少，但它的确是一个定值）
>
>    - 而第一项和第二项可以合并成一个KL Divergence的散度表达式，同时因为将KL Divergence放在了最后面，所以整个公式符号其实发生了变化，所以argmin => argmax
>
> 5. 第五行是利用Gaussian Distribution的定义得到的，即我们前文假设$p(x|z)$是一个标准Gaussian Distribution，代入概率密度函数即可得到，「其中$c$是一个参数」

根据前文内容，我们在这里**梳理一下思路**

1. 作者假设了分布$p(z)$和$p(x|z)$均为标准高斯分布，我们可以通过Bayes定理得到$p(z|x)$
   1. $p(z)$只是一个简单的高斯模型，我们无须过多的讨论它
   2. $p(x|z)$是一个高斯分布，但是具有两个参数$c$和$f$，分别决定Distribution的Covariance和Mean

2. 但我们又知道，Bayes定理我们很难直接求解$p(z|x)$，因为$p(x)$较为难求
3. 所以作者转换思路，不进行求解$p(z|x)$，而是用一个Distribution $q_x^*$去近似$p(z|x)$
4. 在上面的推导过程中，我们发现作者计算KL Divergence时，将$p(z|x)$用Bayes公式代替，并求对数，这样最后就会分离出一个$E_{z\text{~}q_x}log(p(x))$，这个东西为常数，所以就**避免了$p(x)$难以求解的问题**「所以本质上是，我们很难求解$p(x)$，但我们可以很好的求解他的近似分布$q_x^*(z)$的相关参数」

综上，作者目前为止的思路就是，**对于给定的输入x，当我们从近似分布$q_x^*(z)$采样z和从分布p(x|z)采样$\hat{x}$时，希望最大化$\hat{x}==x$的概率**，因此我们正在寻找最优的$f^*$
$$
f^*=\arg\max_{f\in F}E_{z\text{~}q_x^*}logp(x|z) = \arg\max E_{z\text{~}q_x^*}(-\frac{||x-f(z)||^2}{2c})
$$
我们之前有个这样的公式
$$
(g^*,h^*) = \arg\max_{(g,h)\in G\times H}(E_{z\text{~}q_x}(-\frac{||x-f(z)||^2}{2c})-KL(q_x(z),p(z)))
$$

---

合并我们可以得到，我们的目标，即寻找最优的$f^*,g^*,h^*$
$$
(f^*,g^*,h^*) = \arg\max_{(f,g,h)\in F\times G\times H}(E_{z\text{~}q_x}(-\frac{||x-f(z)||^2}{2c})-KL(q_x(z),p(z)))
$$

1. $||x-f(z)||^2$可以理解为$x$和$f(z)$之间的Reconstruction Error，作为Auto-encoder我们希望他肯定是尽可能的小
2. $q_x(z)$和$p(z)$之间的KL Divergence，体现了我们近似分布和真实分布之间的差异，相当于一个**正则项**
3. 参数c
   1. c越高 => 代表$f(z)$周围的Covariance越大 => 我们越关注正则项
   2. c越小 => 代表$f(z)$周围的Covariance越小 => 我们就不怎么关注正则项

#### 3.3 Bringing Neural Networks into the Model

我们的优化目标已经确定了
$$
(f^*,g^*,h^*) = \arg\max_{(f,g,h)\in F\times G\times H}(E_{z\text{~}q_x}(-\frac{||x-f(z)||^2}{2c})-KL(q_x(z),p(z)))​
$$
所以我们可以开始用神经网络开始建模，并对参数进行优化

作者对神经网络进行了定义，如下
$$
\mu_x = g(x) = g_2(g_1(x)), \sigma_x = h(x) = h_2(h_1(x)),g_1(x)=h_1(x)
$$

##### 3.3.1 Encoder

![[Attachment/Pasted image 20201130151138.png]]

1. $g$和$h$并非两个独立的网络，而是有一些共享的Structure和Weight
2. 为了简化计算并减少Parameter的数量，作者做了额外的假设，认为$p(z|x)$的近似Distribution $q_x(z)$是Diagonal Covariance Matrix
3. 在此假设下，相当于减少了我们用于变分推断的分布族，因此对$p(z|x)$的近似可能会不太准确

##### 3.3.2 Decoder

![[Attachment/Pasted image 20201130151144.png]]

1. $f$函数由神经网络建模

---

我们将Encoder和Decoder串联在一起可以得到总体架构，但是会有一些问题。

1. Encoder会产生一个$\mu,\sigma$，根据这两个参数我们可以确定一个Distribution
2. 之后我们将会从这个Distribution中进行采样Sample
3. 但是我们为了更新参数，**采样过程**必须允许误差通过网络反向传播，但其实Sample操作是**不可导的**

为了解决上述问题，可以采用一个**重参数化技巧（Reparametrisation Trick）**，这样就可以让Gradient Descent成为可能。如下：

如果$z$是遵循$mean = g(x), covariance = h(x)$的Gaussian Distribution的随机变量，则$z$可以表示为
$$
z = h(x)\zeta+g(x),\zeta \equiv N(0,1)
$$
![[Attachment/Pasted image 20201130151153.png]]

1. 这个技巧很巧妙，通过Sample一个实系数来获得对高维向量的采样，这样整个过程就可导
2. 如果没有重采样技巧，我们需要从一个真实的分布中采样一个高维向量，这是不可导的；加入重采样技巧，我们将可以直接对均值和协方差向量进行反向传播

---

![[Attachment/Pasted image 20201130151159.png]]

考虑到3.3节我们推断出如下公式
$$
(f^*,g^*,h^*) = \arg\max_{(f,g,h)\in F\times G\times H}(E_{z\text{~}q_x}(-\frac{||x-f(z)||^2}{2c})-KL(q_x(z),p(z)))​
$$
作者在这里用$C$表示$1/(2c)$，其中理论期望用[[Monte-Carlo Approximation]]近似代替，从而得到损失函数公式如图
$$
loss = C||x-\hat{x}||^2+KL[N(\mu_x,\sigma_x),N(0,I)] \\= C||x-f(z)||^2+KL[N(g(x),h(x)),N(0,I)]
$$
损失函数由三部分组成

1. Reconstruction Error
2. 正则项（KL Divergence）
3. 常数C来定义前两项的相对权重

### 4. Summary

1. Auto-encoder是Encoder和Decoder组成的NN结构，通过创建Bottleneck（也就是中间的Latent Space）来处理数据，并经过训练在Encode和Decode过程中损失最少的信息（通过Gradient Descent进行训练，尽可能减少Reconstruction Error）

2. 由于Overfitting，Auto-encoder的Latent Space可能非常不规则

   1. 临近点可能产生截然不同的解码数据
   2. 解码后，Latent Space中的某些点可能产生无意义的内容

   因此，我们无法定义Generative Process（从Latent Space中Sample Single Point，并通过Decoder返回新内容，这在AE中是不可能的）

3. VAE不返回Single Point而返回Distribution，并在Loss Function中加入一个KL Divergence正则项来解决Latent Space不规则的问题
4. **假设**有一个简单的基本概率模型可以用来描述我们的数据，则可以使用Variational Autoencoders详细推导VAE Loss Function

#### 5. VAE & GAN

1. GAN对抗训练的概念简介
2. VAE的理论基础（Probabilistic Model和Variational Inference）的复杂程度较高