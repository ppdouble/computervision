#### 网络结构

原始论文 [http://www.cs.toronto.edu/~fritz/absps/imagenet.pdf](http://www.cs.toronto.edu/~fritz/absps/imagenet.pdf)

2012年ImageNet 竞赛第一，远比第二名领先，标志着DNN深度学习革命的开始。

特点：5个卷积层+3个全连接层

60M个参数+650K个神经元

2个分组->2个GPU(3GB、2块GTX 580 训练5至6天)

引入的新技术 ReLU、Max polling、Dropout regularization

LRU 相邻通道kernel上同一位置的数值进行归一化（VGGNet的论文认为没什么用）

网络结构如图:

[vggnet](vgg.png)

为了后面为整数，前面做成 227\*227

| 输～～～～～～~入 <br>$227\times 227\times 3$| GPU 1 | GPU 2| 输～～～～～～～～～～～～出|
| -- | -- | -- | -- |
|卷积层 1 <br>$227\times 227\times 3$|卷积核$11\times 11$ 数量48 步长 4 | 卷积核$11\times 11$ 数量48 步长 4 ||
| |激活函数 relu | 激活函数 relu|特征图: $(227-11)/4+1=55$ <br>即 $55\times 55\times 96$|
| | 池化Max pooling,  kernel size 3, stride 2|池化Max pooling,  kernel size 3, stride 2 |特征图:$(55-3)/2+1=27$ <br>即 $27\times 27\times 96$|
| |标准化 Local Response Normalization | ||
|卷积层 2 <br>$27\times 27\times 96$<br> 通道独立<br> 双GPU交互|卷积核$5\times 5$ 数量128 步长 1  |卷积核$5\times 5$ 数量128 步长 1 |输入特征图先扩展padding2个像素<br>即$31\times 31$|
| |激活函数 relu |激活函数 relu |特征图:$(31-5)/2+1=27$ <br>即 $27\times 27\times 256$|
| | 池化Max pooling,  kernel size 3, stride 2| 池化Max pooling,  kernel size 3, stride 2|特征图:$(27-3)/2+1=13$ <br>即 $13\times 13\times 256$|
| |标准化 Local Response Normalization| ||
|卷积层 3 <br>$13\times 13\times 256$<br> 通道合并<br>双GPU交互|卷积核$3\times 3$ 数量192 步长 1  | 卷积核$3\times 3$ 数量192 步长 1 |输入特征图先扩展padding1个像素<br>即$15\times 15$|
| |激活函数 relu |激活函数 relu |特征图:$(15-3)/1+1=13$ <br>即 $13\times 13\times 384$|
|卷积层 4 <br>$13\times 13\times 384$<br> 通道独立|卷积核$3\times 3$ 数量192 步长 1   |卷积核$3\times 3$ 数量192 步长 1 |输入特征图先扩展padding1个像素<br>即$15\times 15$|
| |激活函数 relu |激活函数 relu |特征图:$(15-3)/1+1=13$ <br>即 $13\times 13\times 384$|
|卷积层 5 <br>$13\times 13\times 384$<br> 通道独立|卷积核$3\times 3$ 数量128 步长 1  | |输入特征图先扩展padding1个像素<br>即$15\times 15$|
| |激活函数 relu |激活函数 relu |特征图:$(15-3)/1+1=13$ <br>即 $13\times 13\times 256$|
| | 池化Max pooling, kernel size 3, stride 2| 池化Max pooling, kernel size 3, stride 2|特征图:$(13-3)/2+1=6$ <br>即 $6\times 6\times 256$|
|全连接 6<br>$6\times 6\times 256$|2048个神经元 |2048个神经元 ||
| |dropout |dropout|$4096\times 1$的向量|
|全连接 7 <br>$4096\times 1$|2048个神经元 |2048个神经元 ||
| |dropout |dropout|$4096\times 1$的向量|
|全连接 8<br> $4096\times 1$|1000个神经元 | |$1000\times 1$的向量|

#### 环境
Anaconda3 2021.05

Fedora workstation 29 x86_64

python 3.8.8

tensorflow                2.2.0

scipy                     1.6.2

imageio                   2.9.0

#### tensorflow 2 兼容模式

```python
import tensorflow.compat.v1 as tf
```
