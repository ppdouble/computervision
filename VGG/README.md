#### 网络
ImageNet-2014 竞赛第二，**网络改造的首选基础网络**，典型的层数是19层
提出目的是为了探究在大规模图像识别任务中，卷积网络深度对模型精确度的影响
一个大卷积核分解成连续多个小卷核
例如 $7\times7$ 分解成 3个 $3\times 3$ 核 由ReLU连接，参数数量由$49C^2$ 降至 $27C^2$
VGG可以减少参数，降低计算，增加深度

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
