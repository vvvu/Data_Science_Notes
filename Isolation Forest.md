# Isolation Forest
主要内容参考于这两篇文章[^1][^2]
## Summary
- Isolation Forest is an anomaly detection technique that **identifies anomalies instead of normal observations** - 常规的异常检测都是通过识别正常的观测值，筛除正常的观测值来留下异常点，Isolation Forest是直接去寻找异常点而非筛除正常点
- Similarly to [[Random Forest]], it is built on an [[Ensemble Learning]] of [[Decision Tree]]
- [[Extended Isolation Forest]] => improvement to the Isolation Forest algorithm - Isolation Forest最主要的问题是，在下文提到的Random Split算法中，只能进行与轴的平行分割。（见下文的分割图，分割线只能垂直分割或者水平分割，而不能斜向分割）
## Notes
1. Isolation Forest的随机分割算法，步骤可以总结为  ( Random Feature -> Random Split Value -> A point is isolated | Reach maximum depth )
	1. 随机选取一个feature
	2. 在feature中随机选取一个split value(between min and max value)，低于split value的值向左分割，高于split value的值向右分割
	3. 重复以上过程，直到[ 一个点被隔离[^3]，或者到达指定决策树的最大深度[^4] ]

2. Isolation Forest的前提假设认为：Anomaly Point很少，而且与Normal Point有显著差异。
	- 结论：作者认为，Anomaly Point很容易在随机分割的过程中被分割出来，因为Anomaly Point是非常离群的 => 所以作者认为，Anomaly Point应该在更靠近Decision Tree Root的地方被识别出来（平均路径长度较短，即一个Observation在树上从Root到Leaf Node[^5]的路径较短，这也代表着我们只需要**很少的分割就可以分割出来目标结果。**
	![[Attachment/Pasted image 20201125112408.png]]
	- 解释：以上图为例，我们认为Point 1为一个异常点，显然如果我们Random Split，很容易就将Normal Point和Point 1分割开。Point 1的离群特性让它在Random Split中极易被分割出去。而Normal Point因为聚集在一起，所以如果我们想要得到某个Isolation Normal Point，我们需要和图(b)一样，不断，多次分割来得到结果。这就导致Root到Leaf Node的距离较长
3. Anomaly Score - 基于以上理论推导，可以知道Anomaly Point的Path Length较短，Normal Point的Path Length较长。所以可以定义Anomaly Score公式
	$$
	s(x,n) = 2^{-E(h(x))/c(n)}
	$$
	- 定义：h(x)是the path length of observation x, c(n)是the average path length of unsucessful search in a BST[^6], n是number of external nodes[^7]
	- Each observation is given an **anomaly score** and the following decision can be made on its basis:
		- A score close to 1 indicates anomalies - score接近1，说明2^的指数部分接近0，说明$E(h(x))$接近0，根据前文推断，离Root越近越有可能是Anomaly Point
		- Score much smaller than 0.5 indicates normal observations - score越小，说明$E(h(x))$越大，离Root越远，所以更可能是Normal Point
		- If all scores are close to 0.5 then the entire sample does not seem to have clearly distinct anomalies - 如果所有score都靠近0.5，代表$E(h(x))/c(n)$靠近1，也就是说，对于每个$x$的期望长度和平均长度差不多，说明这Isolation Tree比较平衡，所以这个Sample可能没有clearly distinct anomalies

[^1]:https://towardsdatascience.com/outlier-detection-with-isolation-forest-3d190448d45e
[^2]:https://towardsdatascience.com/outlier-detection-with-extended-isolation-forest-1e248a3fe97b
[^3]:一个点被隔离指的是，在多次细分之后该范围内只有这一个点
[^4]:每次通过Split Value进行分割实则就是在Decision Tree中增加一层深度。在Decision Tree问题中我们一般会规定Maximum Depth以防止Overfitting
[^5]:Decision Tree中，Leaf Node即代表这个节点被识别出来
[^6]:因为Isolation Tree的终止条件和BST的Unsuccessful Search的结构相同，都是搜索到树的Leaf Node终止，所以可以用BST来分析Isolation Tree的平均路径长度
[^7]:这里的External Nodes我觉得可以姑且理解为Leaf Nodes