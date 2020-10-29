---
title: C++ OpenCV中数据结构
date: 2020-10-28 21:06:00
categories: OpenCV
tags:
---

## 基础图像容器Mat

Mat是一个类，由两部分组成：矩阵头（包括矩阵尺寸，存储方法、存储地址等信息）和一个指向存储所有像素值的矩阵的指针。

OpenCV中使用计数机制，让每个Mat对象有自己的信息头，但共享同一个矩阵，这通过让矩阵指针指向同一地址而实现，而拷贝构造函数则只复制信息头和矩阵指针，而不复制矩阵。

如果仍要复制矩阵本身可以使用clone()或copyTo函数

### 创建一个ROI

```c++
Mat D(A,Rect(10,10,100,100));

Mat D = A(Range::all(),Range(1,3));

```

## 创建Mat对象的方法

1. 使用Mat()构造函数`Mat M(2,2,CV_8UC3,Scalar(0,0,255));`(8UC3代表8位unsigned char,3通道，Scalar代表颜色)
2. Matlab式创建方式
   - `Mat E = Mat::eye(4,4,CV_64F);`
   - `Mat E = Mat::zeros(4,4,CV_64F);`
   - `Mat E = Mat::ones(4,4,CV_64F);`
3. 逗号分隔式：` Mat C = (Mat_<double>(3,3)<<0,1,0,1,-1,0,1,1,0)`

## 其他常用数据结构

- Point2f(f代表float) Point2i 二维点 Point与二维点等价
- Point3f Point3i 三维点
- 基于Mat的vector
- vector结合二维点和三维点
- Scalar类 常用来传递像素值如RGB
- Size类 用来表示宽度和高度
- Rect类 Rect(x,y,width,height  )
  
