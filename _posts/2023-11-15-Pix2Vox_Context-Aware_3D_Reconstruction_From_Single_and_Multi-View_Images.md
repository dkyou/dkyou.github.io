---

title: pix2Vox文献总结(Pix2Vox Context-ware 3D Reconstruction From Single and Multi-View Images)
tags: TeXt

---
文献总结
# pix2Vox文献总结(Pix2Vox Context-ware 3D Reconstruction From Single and Multi-View Images)

## 研究背景

​	利用深度神经网络从单视图或者多视图的RGB凸显中恢复对象的三维图形。对象用三维体素(voxel)表示，0表示空，1表示非空。

​	一些主流工作（3D-R2N2）使用递归神经网络的缺点：(1)重建结果与图像序列有关，输出结果不稳定.(2)长期记忆缺失，不能充分利用输入来完善结果。

## 提出方案

​	Pix2Vox。具有两个版本：Pix2Vox-F和Pix2Vox-A

## 组成模块

​	编码器(encoder),解码器(decoder),上下文感知模块(contex-aware)细化器(refiner)。

​	其中，A版本比F版本多了细化器模块。

### 各个模块的具体功能

#### 编码器：

为解码器计算重建三维图形的一组特征

#### 解码器：

将2D特征信息转换为3D

####    上下文感知：

​	自适应地从不同地粗糙3D体积中选择高质量部分重建，是文献中的重要    创新点。为每个粗糙3D体生成一个分数图，然后加权融合。这种方法可以更好地利用    多视图来恢复对象地结构。计算得分图sr采用的标准化函数是softmax()函数。计算融    合体素vf是用得分s和体素相乘再求和。在消融实验中分别去除了上下文感知融合    模块和细化器模块。用3D卷积LSTM代替感知模块，发现在有感知模块的效果优于基 于RNN的效果。去掉细化器之后，Pix2Vox-A的IoU降至0.636，精度明显降低。发 现上下文感知模块和细化器发挥了重要的作用，从而改善了以往的方法的不足。

#### 细化器
​	其原理应该是残差网络，作用是纠正3D体中错误恢复的部分，提高准确性。通过U-net连接，融合体的局部结构可以保留。

### 具体流程

​	编码器从输入图像中生成特征图，送给解码器，解码器生成粗糙三维体。然后单个或多个三维体送到上下文感知模块，上下文感知模块生成一个分数图，根据分数图自适应地选择高质量部分重建。最后细化器生成最终的重建结果。

##  pix2vox优势

### 评估标准

​	iou相似性测量，设置二值化阈值为0.3

### 空间复杂度：

​	与3D-R2N2相比，Pix2Vox-F的参数数量减少了80%

### 时间复杂度：
​	在单视图重建中，Pix2Vox-F和Pix2Vox-A在前向推理方面都比3D-R2N2    快约8倍。在后向推理中，Pix2Vox-F和Pix2Vox-A分别比3D-R2N2快约24倍和4倍。

### 泛化能力：
3D-R2N2的重建IoU为0.120，而Pix2Vox-F和Pix2Vox-A分别为0.209和    0.227
### 在重建合成图像，real-world图像重建方面性能均优
​	文章花了大量篇幅和图表在介绍无论是人工合成图片，还是真实世界图像中的表现，分别对比了几种主流方案，发现pix2Vox的性能在各方面都优于其他方法。
## pix2Vox的不足之处
###  分辨率较低
针对分辨率较低的问题，文献中提出了可以引入GANs的方法
### 没有扩展到RGB-D
未来的工作提到他们计划扩展到RGB-D图像中去重建3D对象
