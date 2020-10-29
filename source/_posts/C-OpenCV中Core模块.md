---
title: C++ OpenCV中Core模块
date: 2020-10-29 10:20:08
categories: OpenCV
tags:
---

## 访问图像中像素

每行像素个数是列数×通道数

Mat类提供ptr函数得到图像任意行的首地址。

`uchar* data = outputImage.ptr<uchar>(i)`

然后访问某个元素就是data[j] 或者是 *(data+j)

**值得注意的是，像素存放顺序是BGR，而不是RGB**

## 计算数组加权和addWeighted()

``` C++
void addWeighted(InputArray src1,double alpha,InputArray src2,double beta,double gamma,OutputArray dst,int dtype=-1);
```

计算得到，dst = src1[I] * alpha + src2[i] * beta +gamma

I是多维数组元素的索引值。而且，在遇到多通道数组的时候，每个通道都需要独立地进行处理。

## 分离颜色通道和通道合并

### 分离颜色通道

``` C++
void spilt(const Mat& src,OutputArrayOfArrays mv);
```

### 通道合并

``` C++
void merge(InputArrayOfArrays mv,OutputArray dst);
```

## 图像对比度

