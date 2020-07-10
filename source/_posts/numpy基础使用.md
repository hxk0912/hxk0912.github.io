---
title: numpy基础使用
date: 2020-07-08 13:23:55
categories: numpy
tags:
---

## 矩阵的创建

``` python
import numpy as np

a = np.array([1,2,3,4,5])

b = np.array(range(1,6))

c = np.arange(1,6,dtype="int8")# dtype是矩阵中每个元素对应的数据类型

print(a)

print(b)

print(c.dtype)

```

### 调整数据类型

`c = c.astype('float32')`

### 保留小数位数

``` python
c = np.round(c,2)
# 保留两位小数
```

## 访问array元素

``` python
a[0] # 第一行
a[:,0]# 第一列
a[0,1:3]# 第一行 2~3列的元素
b=a>3 # 返回bool数组
c=copy(a[a>3])
np.shape(a) # 返回一个元组
```

## array运算

``` python
# 转置
a.T
# 矩阵乘法
np.dot(a,b)
# 点乘
a * b
# 点除
a/b
# 矩阵除法
linalg.solve(a,b)# 对应于a\b
# 求和
a.sum()
# 求逆
linalg.inv(a)
# 特征值
D,V = linalg.eig(a)

```