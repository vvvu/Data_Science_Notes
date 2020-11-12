# Annealing The Learning Rate
## Target 
- In order to make the optimizer **converge faster and closest to the global minimum of the loss function**
## Why We Need This & How To Do This
1. 训练NN时，Learning Rate不宜一直过高，否则Parameter会混乱跳动，难以收敛到使Loss Function更小的部分[^1]
	- Learning Rate衰减太快，系统收敛太快，不一定能达到global minimum local
	- Learning Rate衰减太慢，计算时间较长，很长一段时间Model没有什么改进
2. There are **three** common types of implementing the learning rate decay :
	1. **Step Decay** : Reduce the learning rate by some factor every few epochs
	2. **Exponential Decay** : $\alpha = \alpha_0e^{-kt}$
		- $\alpha$ = learning rate
		- $\alpha_0$ = Initial learning rate
		- $\k$ = A hyperparameter, 可以称为衰减因子
		- $\t$ = Iteration number
	3. **1/t Decay** : $\alpha = \alpha_0/(1+kt)$
		- The meaning of parameters is same as **Exponential Decay**
## Implementation
1. In Keras you can using the `ReduceLROnPlateau` function from `Keras.callbacks`
	```python
	from keras.callbacks import ReduceLROnPlateau
	
	learning_rate_reduction = ReduceLROnPlateau(monitor = 'val_accuracy',
                                           patience = 3,
                                           verbose = 1,
                                           factor = 0.5,
                                           min_lr = 0.00001)
	```
	
	
[^1]: CS231n Convolutional Neural Networks for Visual Recognition https://cs231n.github.io/neural-networks-3/#anneal