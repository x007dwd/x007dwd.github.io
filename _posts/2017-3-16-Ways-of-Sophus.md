---
layout:     post
title:      "Sophus库的使用"
subtitle:   " \"博学之审问之慎思之明辨之笃行之。\""
date:       2017-3-16 21：19
author:     "Bobin"
tags:
    - Sophus
---

在看sophus的代码的时候除了弄懂那些traits技巧，在使用Sophus库的时候应该注意各类之间的继承以及相关的接口。如下图所示。SE3x(这里包括SE3d、SE3f)实例化于SE3Group。
![](/img/Ways-of-Sophus.png)
## 变换
1. SE3类型给出了乘以点，也就是通用变换$\mathbf{R}\mathbf{p}+\mathbf{t}$，这种变换使用一个*重载了这个操作。

# SE3GroupBase
## 输入

|--|功能|函数|
|--|---|---|
|1. | 四元数|setQuaternion()|
|2. | 旋转矩阵 | setRotationMatrix()|
|3. | 仿射矩阵 | setAffine()|
## 矩阵操作

|--|功能|函数|
|--|---|---|
|1. | adjoint Transform | Adj()|
|2. | inverse | inverse()|
|3. | to lie algebra | so3()|
|4. | log map | log()|
|5. | exp map | exp()|
|6. |归一化so3元素|normalize()|
|7. |hat|hat()|
|8. |李括号|lieBranket()|
9.

## 输出

|--|功能|函数|
|--|---|---|
|1. |仿射矩阵| affine3()|
|2. |对应4x4矩阵| matrix()|
|3. |对应3x4矩阵|matrix3x4()|
|3. |对应旋转|rotationMatrix()|
|4. |对应平移|translation()|
|5. |四元数|unit_quaternion()|
6.

## 被重载操作

|--|功能|符号|
|--|---|---|
|1. | 点坐标系变换 | *|
|2. | 赋值 | =|
|3. | 坐标系composition | *|

# SE3Group

该类继承自SE3GroupBase，该类的成员函数主要
1. 构造函数，构造可以来自一个4x4矩阵，也可以来自四元数+位移，还可以是旋转矩阵+位移。
2. 返回数据指针，scalar
3. 返回so3、translation。
