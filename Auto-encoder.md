# Auto-encoder
## Background
Autoencoder is an **unsupervised** artificial neural network that learns **how to efficiently compress and encode data** then learns how to reconstruct the data back from the reduced encoded representation to a representation that is as close to the original input as possible.[^1]
## Autoencoder Components
![[Attachment/Pasted image 20201125201328.png]]
1. Encoder - 将Input压缩成Compressed Representation
2. Bottleneck - 包含Compressed Representation的层，这是Input Data的最低尺寸
3. Decoder - 将Compressed Representation重构出来，得到Output
4. Loss Function - **Reconstruction Loss**，我们期望重构得到的Output和Input尽可能的相似。越相似我们认为当前Auto-encoder的性能越好，对应**Reconstruction Loss**月越低

## Why Do We Design Auto-encoder like this ?
1. **因为我们想要Input和Output尽可能的相似，而我们中间又会涉及到一个Compact的过程。这样就强迫Model在学习的过程中尽可能地提取可以让Input和Output相似的关键特征**
2. **We can use decoder to generate something.** （本质上decoder也是根据Embedding来生成output，所以他当然可以用来生成一些东西）
## Autoencoder Architecture
1. Auto-encoder的体系结构可以在Simple FeedForward network, LSTM network or Convolutional Neural Network等等之间变幻，取决于具体问题
2. Compressed Representation - 我们常常记作**Latent Variable, Embedding, Latent Representation, Latent Code**
3. Synmetric => 我们并不一定要求Auto-encoder是对称的，即如果Encoder中的某一个Layer参数为$W$，并不一定要求Decoder中有一个对应位置的Layer参数为$W^T$。我们想怎么Train这个网络都可以
4. **Deep Auto-encoder** => 中间有很多层的Auto-encoder，主要体现在**NN Encoder**和**NN Decoder**的复杂性上

## Improvement
- De-noising auto-encoder `Input x => Add Noise to x and get x' => Compressed Representation => decode to y`
- Comtractive auto-encoder

## Application
1. Text Retrival
2. Similar Image Search
3. Pre-training DNN
4. Anomaly Detection
5. Sequence-2-Sequence Auto-encoder
### For CNN
1. CNN : `Image => Convolution => Pooling => Convolution => Pooling`
2. Auto-encoder for CNN
	1. `Encoder : Image => Convolution => Pooling => Convolution => Pooling => ... => Compressed Representation`
	2. `Decoder : Compressed Representation => Unpooling => Deconvolution => Unpooling => Deconvolution => ... => Image(Reconstruct)`

## More about Auto-encoder
- More Than Minimizing Reconstruction Error
	- using discriminator
	- sequential data
- More Interpretable Embedding
	- Feature Disentangle
	- Discrete and Structured

[^1]:https://towardsdatascience.com/auto-encoder-what-is-it-and-what-is-it-used-for-part-1-3e5c6f017726