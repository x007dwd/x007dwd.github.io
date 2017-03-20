---
layout:     post
title:      "Point Cloud Library"
subtitle:   " \"博学之审问之慎思之明辨之笃行之。\""
date:       2017-3-18 21：19
author:     "Bobin"
tags:
    - PCL
---

## PointCloud
PointCloud的接口非常类似于vector，+=符号被重载，复制的时候每个点的值都会复制过来。at函数返回了一个ref，getMatrixXfMap返回一个Eigen矩阵，注意pointCloud中的点增加之后，width加一，height保持为1。

## Segment
PCL点云库分为几模块，分别是filter、features、segment、keypoints、registration、kdtree、octree、sampl_consesus、surface、recognition、io、visualization。

PCL的函数调用的模式，一种简单的方式是首先构造点云，将数据传入PointCloud结构，然后构造功能类，如segment，SACSegment，然后初始化配置，最后上传数据，然后执行功能。

我们这里简要介绍segment，SACSegmentation 中定义了多种模型，可以分别检测符合这些模型的点，常用的模型是平面、圆柱等。

```c++
SACMODEL_PLANE,
SACMODEL_CYLINDER,
SACMODEL_NORMAL_PLANE,
```
![](/img/PCL-Start-Segment.png)

### Sample Consensus
SAC segment封装了多种分割方法，使用一种方法使用sample consensus 方法提取点云中符合模型的点。SACSegment中定义了一个SACmodel，以及一个SAC，前者用来管理点云的几何模型，后者管理sampl_consesus中使用的方法，包括 SAC_RANSAC，SAC_LMEDS（LeastMedianSquares），SAC_MSAC（MEstimatorSampleConsensus），SAC_RRANSA（RandomizedRandomSampleConsensus），SAC_RMSAC（RandomizedMEstimatorSampleConsensus），SAC_MLESAC（MaximumLikelihoodSampleConsensus），SAC_PROSAC（ProgressiveSampleConsensus）;

SACMODEL_CYLINDER需要使用Normal，在类SACSegmentationFromNormals中实现。
PCL只看网页上的关于接口的介绍，不能明白内部的实现机制，写代码的时候总是在怀疑这样用是不是不太，还是看一看源代码，弄清函数的内部i情况。更容易写出好的实现。



### outlier
分割的模式中一般存在使用Normal或者不使用，在分割之前使用StatisticalOutlierRemoval可以滤掉外点。outlier的处理一般在segment之前，这些函数位数filter类。

### indices
功能类大都继承自pcl_bae类的，可以直接调用函数setIndices。PCLbase类中只是包含了PointCloud的指针，在上传数据的时候，只是将指针传了他，没有数据拷贝的过程。
PointIndices中的数据为std::vector<int>类型的，setIndices就是把这个vector的指针传递过去。

### ModelCoefficients
数据为std::vector<float>，不同的segment模型可以计算对应的模型参数然后使用指针返回。
