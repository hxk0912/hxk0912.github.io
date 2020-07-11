---
title: scipy基础用法
date: 2020-07-11 15:10:40
categories: scipy
tags:
---

## 常量模块

为了方便科学计算，SciPy 提供了一个叫 `scipy.constants` 模块，该模块下包含了常用的物理和数学常数及单位。

``` python

from scipy import constants

constants.pi # pi

constants.c #光速

constants.h # 普朗克常数

```

## 线性代数

线性代数应该是科学计算中最常涉及到的计算方法之一，SciPy 中提供的详细而全面的线性代数计算函数。这些函数基本都放置在模块 `scipy.linalg` 下方。其中，又大致分为：基本求解方法，特征值问题，矩阵分解，矩阵函数，矩阵方程求解，特殊矩阵构造等几个小类。

比如最小二乘法求解函数`scipy.linalg.lstsq`

## 插值函数

方法： `scipy.interpolate`

插值实例

``` python

from matplotlib import pyplot as plt

import numpy as np

from scipy import interpolate

x = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

y = np.array([0, 1, 4, 9, 16, 25, 36, 49, 64, 81])

xx = np.array([0.5, 1.5, 2.5, 3.5, 4.5, 5.5, 6.5, 7.5, 8.5])

f= interpolate.interp1d(x,y)

yy = f(xx)

plt.scatter(xx,yy,marker='*',color='red')

plt.scatter(x,y)

plt.show()
```

## 图像处理

## 优化方法

SciPy 提供的 `scipy.optimize` 模块下包含大量写好的最优化方法。例如上面我们用过的 `scipy.linalg.lstsq` 最小二乘法函数在 `scipy.optimize` 模块下也有一个很相似的函数 `scipy.optimize.least_squares`。这个函数可以解决非线性的最小二乘法问题。

`scipy.optimize` 模块下最常用的函数莫过于 `scipy.optimize.minimize`，因为只需要指定参数即可以选择大量的最优化方法。例如 `scipy.optimize.minimize(method="BFGS")` 准牛顿方法。

## 信号处理

## 统计函数

`scipy.stats` 模块包含大量概率分布函数，主要有连续分布、离散分布以及多变量分布。除此之外还有摘要统计、频率统计、转换和测试等多个小分类。基本涵盖了统计应用的方方面面。

## 稀疏矩阵