---
title: C OpenCV图像变换
date: 2020-11-16 20:10:45
categories: OpenCV
tags:
mathjax: true
---

## 基于OpenCV的边缘检测

### 边缘检测的一般步骤

1. 滤波
- 边缘检测的算法主要是基于图像强度的一阶和二阶导数，但导数通常对噪声很敏感，因此必须采用滤波器来改善与噪声有关的边缘检测器的性能。常见的滤波方法主要有高斯滤波，即采用离散化的高斯函数产生一组归一化的高斯核，然后基于高斯核函数对图像灰度矩阵的每一点进行加权求和。

2. 增强
- 增强边缘的基础是确定图像各点邻域强度的变化值。增强算法可以将图像灰度点邻域强度值有显著变化的点凸显出来。

3. 检测
- 经过增强的图像往往邻域中有很多点的梯度值比较大，而在特定的应用中，这些点并不是要找的边缘点，所以应该采用某种方法来对这些点进行取舍，实际工程中，常用的方法是通过阈值化方法来检测。

### canny算子

#### canny边缘检测步骤

1. 消除噪声，首先使用高斯平滑滤波器卷积降噪，以下是一个高斯内核示例。

$$
K=\frac{1}{139}
\begin{bmatrix}
    2&4&5&4&2\\
    4&9&12&9&4\\
    5&12&15&12&5\\
    4&9&12&9&4\\
    2&4&5&4&2
\end{bmatrix}

$$

2. 计算梯度幅度和方向
3. 非极大值抑制，这一步排除非边缘像素，仅仅保留了一些细线条（候选边缘）
4. 滞后阈值 滞后阈值需要使用两个阈值（高阈值和低阈值）
   1. 若某一像素位置的幅值超过高阈值，该像素被保留为边缘像素。
   2. 若某一像素位置的幅值小于低阈值，该像素被排除
   3. 若某一像素的幅值在两个阈值之间，该像素仅仅在连接到一个高于髙阈值的像素时被保留。

**对于Canny函数的使用，推荐的高低阈值比在2:1到3:1之间**

### 相关API函数

``` C++

void Canny(InputArray image,OutputArray edges,double threshold1,double threshold2,int apertureSize =3,bool L2gradient=false)

```

第三个参数，阈值1
第四个参数，阈值2
第五个参数 apertureSize 表示应用Sobel算子的孔径大小。默认值为3
第六个参数，bool类型L2gradients 一个计算图像梯度幅值的标识，有默认值false

**需要注意的是，阈值1和阈值2两者中较小的用于边缘连接，而较大的用于控制强边缘的初始段**

### Sobel算子

sobel算子是一个主要用于边缘检测的离散微分算子，他结合了高斯平滑和微分求导，用来计算图像灰度函数的近似梯度，在图像的任何一点使用此算子，都将会产生对应的梯度矢量或是其法矢量。

### sobel算子的计算过程

1. 分别在x和y两个方向求导
    1. 水平变化：将图像I与一个奇数大小的内核G<sub>x</sub>进行卷积。比如：当内核大小为3时，G<sub>x</sub>的计算结果为
   $$
    G_x = \begin{bmatrix}
        -1&0&1\\
        -2&0&2\\
        -1&0&1
    \end{bmatrix} * I
   $$
    2. 垂直变化：将I与一个奇数大小的内核进行卷积。比如：当内核大小为3时，G<sub>y</sub>的计算结果为：
   $$
    G_y = \begin{bmatrix}
        -1&-2&-1\\
        0&0&0\\
        1&2&1
    \end{bmatrix} * I
   $$
2. 在图像的每一点，结合以上两个结果求出近似梯度：
   $$
    G=\sqrt{G_x^2 + G_y^2}
   $$
3. 使用sobel算子
``` C++
void Sobel(InputArray src,OutputArray dst,int ddepth,int dx,int dy,int ksize=3,double scale=1,double delta=0,int boardType=BORDER_DEFAULT)
```

ddepth 代表输出图像的深度，填-1即可

dx dy代表在x，y方向上的差分阶数

ksize表示核的大小。

scale表示计算导数值时的缩放因子，默认为1

delta表示结果存入目标图前，可选的增量值，默认为0

### Laplacian算子

同数学上的定义

$$
Laplacian(f) = \frac{\partial^2f}{\partial x^2}+\frac{\partial^2f}{\partial y^2}
$$

同时由于Laplacian使用了图像梯度，其内部代码实际上调用了Sobel算子。

**让一幅图像减去它的Laplacian算子可以增强对比度**

使用Laplacian算子
``` C++
void Laplacian(InputArray src,OutputArray dst,int ddepth,int ksize=3,double scale=1,double delta=0,int boardType=BORDER_DEFAULT)
```

### scharr滤波器

我们一般直接称scharr为滤波器而不是算子，它主要是配合Sobel算子的运算而存在的。因为当内核大小为3时，sobel算子可能会产生比较明显的误差。为解决这一问题，opencv提供了scharr函数，但该函数只作用于大小为3的内核，该函数结果更加精确。

内核如下：

$$
G_x = \begin{bmatrix}
        -3&0&3\\
        -10&0&10\\
        -3&0&3
    \end{bmatrix} * I
   $$

   $$
    G_y = \begin{bmatrix}
        -3&-10&-3\\
        0&0&0\\
        3&10&3
    \end{bmatrix} * I
   $$

使用scharr算子
``` C++
void Scharr(InputArray src,OutputArray dst,int ddepth,int dx,int dy,double scale=1,double delta=0,int boardType=BORDER_DEFAULT)
```

相比sobel，scharr仅少了ksize项。
