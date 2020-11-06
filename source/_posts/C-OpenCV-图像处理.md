---
title: C OpenCV 图像处理
date: 2020-11-05 14:22:30
categories: OpenCV
tags:
---

## 图像滤波与滤波器

图像滤波，指在尽量保留图像细节特征的条件下对目标图像的噪声进行抑制。

消除图像中的噪声成分叫做叫做图像的平滑化或者滤波操作。信号或图像的能量大部分集中在幅度谱的低频和中频段，而在较高频段，有用的信息经常被噪声淹没。因此一个能降低高频成分幅度的滤波器就能减弱噪声的影响。

opencv中提供了常见的五种滤波器：

- 方框滤波 BoxBlur
- 均值滤波 Blur
- 高斯滤波 GaussianBlur
- 中值滤波 medianBlur
- 双边滤波 bilateralBlur

## 滤波和模糊

滤波可以分为高通和低通，高斯滤波是指用高斯函数作为滤波函数的滤波操作，至于是不是模糊，要看是高斯低通还是高斯高通，低通就是模糊，高通就是锐化

## 线性滤波

### 方框滤波

``` C++

void boxFilter(InputArray src,OutputArray dst,int ddepth,size ksize,Point anchor = Point(-1,-1),boolnormalize = true,int borderType=BOARDER_DEFAULT);

```

size 代表内核大小 3×3，5×5等，anchor指平滑锚点，默认值为中间，normalize表示是否归一化，true的时候代表归一化，也就是**均值滤波**，不归一化是用于计算像素邻域内的积分特性。

### 高斯滤波

高斯滤波就是对整幅图像进行加权平均。用一个具体模版（掩膜）扫描图像中的每一个像素，用模版确定的领域内扫描图像中的每一个像素，用模版确定领域内像素的加权平均灰度值去替代模版中心的像素值。

``` C++
void GaussianBlur(InputArray src,OutputArray dst,Size ksize,double sigmaX,double sigmaY=0,int BORDER_DEFAULT)
```

## 非线性滤波

### 中值滤波

中值滤波是用像素点邻域灰度值的中值来代替该像素点的灰度值。

中值滤波与均值滤波比较：在均值滤波器中，用于噪声成分被放入平均计算中，所以输出受到了噪声的影响。但是在中值滤波器中，由于噪声成分很难被选上，所以几乎不会影响到输出。因此，中值滤波的消除噪声能力更胜一筹，中值滤波无论是在消除噪声还是保存边缘上都是一个不错的方法。但是中值滤波花费的时间是均值滤波的5倍以上。

``` C++
void medianBlur(InputArray src,OutputArray dst,int ksize)

```

### 双边滤波

双边滤波的好处，是可以做边缘保存，双边滤波相比于高斯滤波多了一个高斯方差sigma-d，它是基于空间分布的高斯滤波函数，所以在边缘附近，离得较远的向世俗不会对边缘上的像素值影响太多。但是，由于保存了过多的高频信息，对于彩色图像里的高频噪声，双边滤波器不能干净的滤掉，只能对于低频信息进行较好的滤波。简单来说，双边滤波中不仅考虑掩膜中点对于中心距离的影响，也考虑点本身像素与中心像素值的差的影响。

``` C++
void bilateralFilter(InputArray src,OutputArray dst,int d,double sigmaColor,double sigmaSpace,int boarderType=BORDER_DEFAULT)
```

d代表过滤过程中邻域像素的直径。
sigmaColor 代表颜色空间的sigma值，sigmaSpace代表坐标空间的sigma值

## 形态学滤波

### 形态学概述

形态学操作就是基于形状的一系列图像处理操作。

### 腐蚀

腐蚀就是求局部最大值的操作，