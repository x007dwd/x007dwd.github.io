---
layout:     post
title:      "Eigen库的使用"
subtitle:   " \"博学之审问之慎思之明辨之笃行之。\""
date:       2017-3-16 21：19
author:     "Bobin"
tags:
    - Eigen
---
Eigen库使用比较多的模板的内容，因此在编译的时候，错误提示比较complicated。有时候一个错误会导致长长的编译错误，排错、debug因此更加困难。

## 注意点
1. Eigen::Matrix<type, Num, Num>。type表示类型，Num表示矩阵的行和列的大小，Num可以是动态指定的，也可以预先设定好。主要的点是type在进行数据传递的时候，左值和右值的type需要匹配，不然编译报错，即使是float赋值给double，这一点常用的C++中浮点数float、double的处理不太一样。如果两边的type不一下样可以使用cast函数进行转换。
2. Transform< Scalar, Dim, Mode, \_Options> 这个类型中Dim表示空间变换的维度，该类型使用的矩阵的维数为Dim+1，Sophus中使用Eigen::Transform<Scalar, D, Eigen::Affine>作为SE3 函数affine3的返回值。
## 常用的函数
1. 类似于OpenCV，对于矩阵，我们需要一些单位矩阵使用函数setIdentity()。
2. 读取矩阵中的某一块Matrix.block(row_start,col_size,row_end,col_size)，读取某一行使用row(i)，读取某一列使用col(i)。
