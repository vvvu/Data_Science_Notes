# Data Augmentation - How to use Deep Learning When You Have Limited Data
## Tips
- 适用场景：一般应用于Computer Vision领域，即涉及图像处理和计算机视觉
## Q & A
1. Q : Why do we need more data
	A : State-of-the-art neural networks typically have parameters in the order of millions! So **if you have a lot of parameters, you would need to show your machine learning model a proportional amount of samples**[^1]
2. Q : Where do we augment data in our ML pipeline
	**We usually before we feed the data to the model**

|                      | Offline Augmentation                                    | Online Augmentation                        |
| -------------------- | ------------------------------------------------------- | ------------------------------------------ |
| Preferences Datasets | Smaller datasets                                        | Larger datasets                            |
| Method               | To perform all the necessary transformations beforehand | To perform transformations on a mini-batch |

3. Q : How do I get more data if I don't have more data
	A : Popular Augmentation Techniques

## Polpular Augmentation Techniques
- Flip | Rotation | Scale | Crop | Translation | Gaussian Noise | Local Warping (局部变形，调整)
- Advanced Augmentation Techiques : Conditional GANs

## Implementation
```python
'''
Data Augmentation
1. 本质上就是数据扩充，想办法从已有的Dataset中生成更多的Dataset
2. 从而提供更多的Training Data，从而提高准确度，同时避免Overfitting
3. Some popular augmentations : grayscales, horizontal flips, vertical flips, random crops,
                                color jitters, translations, rotations and much more.
4. 在MNIST上的效果
- Without data augmentation - 98.11%
- With data Augmentation - 99.67%
'''
from keras.preprocessing.image import ImageDataGenerator
# With data augmentation to prevent overfitting
datagen = ImageDataGenerator(
    featurewise_center=False,  # set input mean to 0 over the dataset
    samplewise_center=False,  # set each sample mean to 0
    featurewise_std_normalization=False,  # divide inputs by std of the dataset
    samplewise_std_normalization=False,  # divide each input by its std
    zca_whitening=False,  # apply ZCA whitening
    # randomly rotate images in the range (degrees, 0 to 180)
    rotation_range=10,
    zoom_range=0.1,  # Randomly zoom image
    # randomly shift images horizontally (fraction of total width)
    width_shift_range=0.1,
    # randomly shift images vertically (fraction of total height)
    height_shift_range=0.1,
    horizontal_flip=False,  # randomly flip images
    vertical_flip=False)  # randomly flip images

'''
For the data augmentation

- 将一些训练图像随机旋转10度
- 将一些训练图像随机放大10％
- 水平随机移动图像宽度的10％
- 垂直随机移动图像高度的10％
- 没有应用vertical_flip或horizontal_flip，因为它可能导致对对称数字（例如6和9）进行错误分类。

模型准备就绪后，就可以拟合训练数据集。
'''
datagen.fit(X_train)
```
	
[^1]: Data Augmentation | How to use Deep Learning when you have Limited Data — Part 2 https://medium.com/nanonets/how-to-use-deep-learning-when-you-have-limited-data-part-2-data-augmentation-c26971dc8ced