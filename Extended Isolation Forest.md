# Extended Isolation Forest
> A recent improvement to the [[Isolation Forest]] algorithm, namely Extended Isolation Forest(EIF), which addresses major drawback of the orginal method.

## Summary
1. Improve the drawback of Isolation Forest => Cuts that are not parallel the axes
2. Effiency - On the author's Macbook Pro the `sklearn` `IF` took 14s to train, while the `ELF` implementations took roughly 10 minutes

## The Drawback of Isolation Forest
- **Isolation Forest最主要的缺点是：Random Split算法只能水平|垂直切割特征。这样会导致一些后果，见下文**
![[Attachment/Pasted image 20201125122420.png]]
- 如该图所示，Anomaly Score Map可以很好反应Normally Distributed Data，即呈现一种以Central Point (0,0)为中心的分布。
- 但是如果我们有**两个中心**，情况就会出现一些问题
![[Attachment/Pasted image 20201125122634.png]]![[Attachment/Pasted image 20201125122711.png]]
- 因为我们的线只能进行水平和垂直分割，水平和垂直分割的密集程度会导致Anomaly Score的Distribution。在第一种情况下无伤大雅，在第二种情况下，我们的两个中心对应(0,10)和(10,0)两个点，附带着会导致(0,0)和(10,10)两个点的Anomaly Score也有所提升，**这都是Random Split Algorithm只能水平|垂直切割造成的**

## How to Improve The Isolation Forest
1. Extended Isolation Forest不再遵循Isolation Forest的算法步骤[^1]
2. EIF算法选择 => 随机斜率和随机截距 => **从而进行不平行于轴的切割**
	- The **random slope** for the branch cut
	- **random intercept** chosen from the range of available values from the training data
3. 针对前文提到的问题，ELF的切割效果如下
![[Attachment/Pasted image 20201125123316.png]]


[^1]:Random Feature -> Random Split Value -> Leaf Node | Maximum Depth