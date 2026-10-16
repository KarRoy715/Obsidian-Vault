---
onenote-id: 0d67f360-8d18-0807-061b-ed2999c58bca:1
---
# 一、自监督学习

1. **自监督**
- 基于前置任务（pretext task)：其label可以从数据本身创造/生成

1.1 位置预测

**（****1****）相对位置**

![[手写AI image.png]]

Patch1和patch2**共享权重**，作为输入，预测patch2相对patch1的位置

**（****2****）拼图**

![[手写AI image 1.png]]

让模型学习如何将被打乱的图像块重新拼凑成原始图像

1.2  旋转预测

![[手写AI image 2.png]]

让图像选择0、90、180、270四种方式，让模型预测旋转的是哪一种（分类任务），<u>==🟡让模型学习眼睛、嘴巴、尾巴等特征？？？==</u>

1.3 上色

灰度图进行上色，上色更利于模型学习细粒度的特征，因为更关注于细粒度信息

1.4 聚类预测

![[手写AI image 3.png]]

首先通过CNN得到特征，根据特征今昔聚类，给每个类别打上“假标签"，让模型预测假标签，每一次都用更新的CNN进行聚类重新达标，和半监督/若监督学习很像

- 基于对比学习

数据增强常用方法

![[手写AI image 4.png]]

2.1 CLIP

1. 使用了4亿对图文数据对（数据质量很高，互联网公开数据）
1. 模型总览

![[手写AI image 5.png]]

（1）这里的(2)是把imagenet上的文本都变成一句话，为了和训练的时候保持对齐，因为训练的时候的图文对中的文本都是一句话，其次没有上下文可能有歧义（比方boxer可能是狗的一个种类或者是拳击运动）

（2）再经过Text Encoder得到每个文本的特征，

（3）当给到一张图片的是，将图片经过Image Encoder后与所有文本特征进行相似度计算，返回最相近的文本特征对应的句你 子

1. GFT300milion Instagram数据集 3.5billion

2.2 SimCLR

(A simple framework for contrastive learning of visual representations)

![[手写AI image 6.png]]

这里计算loss的时候，使用的是经过MLP映射之后的loss，而下游任务是使用MLP之前的特征表示，因为映射前的特征应该保留了更多图像上的特征

论文消融实验：

使用随机裁剪和抖动可以获得更好的效果

Batch_size更大可以获得更好的效果，因为其中包含更多的负样本，更大的模型，更多训练时间越好

2.3 MoCo

Momentum contrast

![[手写AI image 7.png]]

这里的key是经过query数据增强后得到的，encoder起始时一样的，但是梯度更新的方法不一样，encoder是根据对比损失更新的，而momentum不直接参与对比loss对他的影响，而是基于右下角的这个公式θq是encodeer参数，θk是momentum encodeer参数，m是超参（近似1），因为近似1，所以更多是在自己基础上更新，收到一点点encoder的影响，即受到一点点对比损失函数的影响

- 基于掩码重建（Masked Image Modeling，MIM）

BEiT

![[手写AI image 8.png]]

# 二、视觉大模型

1. **模型综述**

1.1 ViT

![[手写AI image 9.png]]

1.2 MoCo v2

![[手写AI image 10.png]]

1.3 MoCo v3

创新1：使用更大的batch_size，不再使用内存银行

创新2：双向的对比loss（这里的x1和x2是同一个数据，分别经过数据增强得到的）

![[手写AI image 11.png]]

1.4 DINO

![[手写AI image 12.png]]

1.5 MAE

创新1：mask75%patch，

创新2：encoder和decoder非对称，decoder模型更小。输入encoder的只有未掩码部分，这也减小了显存占用。

![[手写AI image 13.png]]

1.6 SAM

![[手写AI image 14.png]]

# 三、多模态大模型

1. **概述：**类型1：输入输出模态不同

类型2：多模态输入

类型3：多模态输出

1. **多模态网络的要素**

Encoder：针对每个模态的encoder

Align strategy：不同模态的对齐/融合方式

LLM（Optional）：以大预言模型为核心的网络

![[手写AI image 15.png]]

1. **模型讲解**
- **CLIP**

![[手写AI image 16.png]]

- **GLIP**

![[手写AI image 17.png]]

上面分支处理文本，下面分支处理图像

CLIP输入的是一批图像一批文本，而GLIP输入的是一张图像和一个文本的子部分

- **Flamingo**

![[手写AI image 18.png]]

**这种以****LLM为核心****的VLM是现在多模态大模型的主流范式**

- **LLaVA**

# 四、下游任务迁移

1. **视觉基础模型给的迁移方法**

![[手写AI image 19.png]]

**1.1** **Linear** **Probing线性探测**

![[手写AI image 20.png]]

**只训练分类器，其他是冻结的**

**1.2 (Full)****Finetune**

![[手写AI image 21.png]]

**1.3** **Adapter**

**1.4** **Prompt**