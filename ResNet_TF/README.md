#### 网络
论文[https://arxiv.org/abs/1512.03385v1](https://arxiv.org/abs/1512.03385v1)

提出背景：网络的层数不能通过一直堆叠来获得好的学习效果。过多的层数会导致梯度消失、梯度爆炸、网络退化（误差增大、效果不好、过适应等问题）。

ResNet主要思路：
残差映射 $F(x)=H(x)-x$，$x$是卷积之前，$H(x)$是卷积之后。$F(x)=H(x)-x$ 相当于学到的东西与原始的东西的差距，相当于卷积没有获取的特征，或者相当于没有学到的内容。


ResNet特点：

(1) 残差映射(主要贡献)

(2) 全是$3\times 3$卷积核(应该是为了操作残差映射)

(3) 卷积步长2取代池化, 没有池化(应该是为了操作残差映射)

(4) 使用Batch Normalization

(5) 取消Max池化，取消全连接层(参数多计算量大，取消掉是趋势，有其它方法替换)，取消Dropout(应该是为了操作残差映射)


优化点：

通过$1\times 1$ 卷积降低计算量，在两头加，前面降维，后面升维，使其与之前维数相同以便于操作$H(x)=F(x)+x$

101层比较常见。另外GoogleLeNet中Inception层可以结合resnet的残差映射. 

#### 环境
Anaconda3 2021.05

Fedora workstation 29 x86_64

python 3.8.8

tensorflow                2.2.0

tf_slim         1.1.0

#### tensorflow 2 兼容模式

```python
tf.compat.v1.
slim.get_or_create_global_step()
```
