---
title: Video Understanding-视频理解
date: 2022-08-12
categories:
- Reading notes
- Video

tags:
- video understanding

description: 视频理解相关知识学习
---
相关文献可具体参考：[Video Action Understanding: A Tutorial](https://arxiv.org/pdf/2010.06647v1.pdf)


# Introduction
----------------------------------------------------------
&emsp 视频理解是一个很复杂的任务, 主要围绕对语义方面的识别, 其中包括场景和环境, 实体对象, 动作, 事件, 属性和概念等, 通常视频理解也可称为semantic video understanding.
涉及到具体的任务, 视频理解又可以大致分为以下几个方面：
- 1.) video classification, 主要是对视频中的动作进行分类 
- 2.)Temporal Action Dection 识别出视频动作中的起始帧和结束帧 
- 3.)video caption 给视频配上文字说明 
- 4.)video grounding 给定一句自然语言描述, 定位视频中的具体位置等

## Optical Flow
--------------------------------------------------------------

&emsp Optical flow or optic flow is the pattern of apparent motion of objects,  surfaces,  and edges in a visual scene caused by the relative motion between an observer and a scene.Optical flow can also be defined as the distribution of apparent velocities of movement of brightness pattern in an image.(wiki)
光流主要是由于观察者和物体之间的相对运动, 引起视觉场景中的物体, 表面, 边缘表观运动的模式.光流可以用来描述物体的运动.
光流技术的主要应用方向在运动估计(motion estimation)和视频压缩(vide compress).在视频领域, 光流常常用在物体检测和跟踪(object detection and tracking), 运动检测(movement detection), 图像主平面抽取(image dominant plane extraction)等.

# Paper Reading
--------------------------------------------------------------

### Two-Stream Convolutional Networks for Action Recognition in Videos(2014)

双流网络的模型结构如下：
<figure>
<a><img src="{{site.url}}/pictures/vd_img.png"></a>
</figure>

主要包含两个部分：空间流卷积和时序流卷积. 想法其实现在来看比较简单, CNN能够提取2D空间图像特征, 但是感知时序的信息比较差. 既然不能
很好的处理时序信息, 那就把提供抽取好的时序信息喂给模型, 然后融合. 

**Optical flow ConvNets**

光流卷积网络：跟图像卷积网络类似, 由于每两个相邻帧就可以构建一张光流图, 表示成张量就是一个$$w\times h \times 2$$的张量（2表示垂直方向和水平方向的分量）. 

具体可以参考下图：图（c）就是蓝色区域的光流图特写. 

<figure>
<a><img src="{{site.url}}/pictures/vd_img_1.png"></a>
</figure>

如何利用这些光流特征：文中作者尝试了两种方式optical flow stacking 和trajectory stacking
<figure>
<a><img src="{{site.url}}/pictures/vd_img_2.png"></a>
</figure>
optical flow stacking 对于每个像素点(u, v), 堆叠当前帧的后L个光流图, 构成一个维度为2L的张量, 用来表示当前点的光流
trajectory stacking 利用光流对像素点进行追踪, 先追踪每个像素点的光流, 在进行合并. 具体可以参考上图. 
虽然第二种方式用来构建光流网络的输入更有启发性, 但是两种方案的效果性能相当. 

早期关于3维卷积网络的研究, 双流网络最致命的缺点就是慢, 抽取光流的算法是非常耗费时间的. 

### Learning Spatiotemporal Features with 3D Convolutional Networks(C3D 2015)

2D卷积和3D卷积的简单示例：
<figure>
<a><img src="{{site.url}}/pictures/vd_img_3.png"></a>
</figure>
为什么2D卷积不能很好的提取空间特征：采用多帧视频, L帧看成L个通道, 二维卷积在第一次提取特征过后, 
时序信息就已经完全（collapsed completely）坍缩, 后续的卷积操作无法再次感知到时序特征

模型结构非常简单
<figure>
<a><img src="{{site.url}}/pictures/vd_img_4.png"></a>
</figure>
主要结论：
- 1.3D卷积能够更好的建模形状和运动特征
- 2.使用$$3\times 3\times 3$$能够达到最好的效果

### Quo Vadis,  Action Recognition? A New Model and the Kinetics Dataset

双流膨胀卷积网络：Two Stream Inflated 3D ConvNet
针对现有的特征提取方案之间深入分析和对比：
<figure>
<a><img src="{{site.url}}/pictures/vd_img_5.png"></a>
</figure>

1.ConvNet+Lstm 利用卷积网络提取视频每一帧特征, 再利用lstm构建时序信息

这种方法的优点是可以利用预训练的2D网络权重, 单独抽取每一帧的特征, 缺点也很显然, lstm是基于高维的时序特征进行建模, 缺少低维的特征. 

2.C3D 直接利用三维卷积网络

三维卷积虽然能够提取时空特征, 但是参数量巨大, 模型训练困难

3.Two-Stream Networks 传统的双流网络

参考上图(c)和(d), 两个不同流的特征融合不充分. 

4.Two-Stream Inflated 3D ConvNets

利用在imagenet上预训练好的模型, 将模型从2D架构改造为3D架构, 将2D的卷积算子和池化算子扩充为3为, 由 $$N\times N$$ 膨胀为 $$N\times N\times N$$, 那么怎么改造已经做好预训练的模型权重呢？它这里面先将2D的卷积核在时间维度上复制N份, 然后再除以N, 再时间维度上归一化, 保证预训练模型数据分布的一致性. 第二步设置时间维度, 空间维度的步长, 池化窗口等超参. 这里简单说明一下为什么需要调整这些参数, 通常的二维卷积对图像的两个维度（垂直方向和水平方向）是平等对待的, 所以池化的核和步长都是相同的. 现在加入了时间维度, 就应该需要考虑到图像的帧率和图像的分辨率. 如果相对空间而言, 时间增长过快, 会破会混合不同对象的边缘特征, 破坏了早期的特征检测, 如果过慢, 又可能不能很好的获取动态场景. 

### Is Space-Time Attention All You Need for Video Understanding(2019)