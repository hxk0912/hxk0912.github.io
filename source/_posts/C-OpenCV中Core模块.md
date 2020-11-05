---
title: C++ OpenCV中Core模块
date: 2020-10-29 10:20:08
categories: OpenCV
mathjax: True
tags:
---

## 访问图像中像素

每行像素个数是列数 × 通道数

Mat 类提供 ptr 函数得到图像任意行的首地址。

`uchar* data = outputImage.ptr<uchar>(i)`

然后访问某个元素就是 data[j] 或者是 \*(data+j)

**值得注意的是，像素存放顺序是 BGR，而不是 RGB**

## 计算数组加权和 addWeighted()

```C++
void addWeighted(InputArray src1,double alpha,InputArray src2,double beta,double gamma,OutputArray dst,int dtype=-1);
```

计算得到，dst = src1[I] _ alpha + src2[i] _ beta +gamma

I 是多维数组元素的索引值。而且，在遇到多通道数组的时候，每个通道都需要独立地进行处理。

## 分离颜色通道和通道合并

### 分离颜色通道

```C++
void spilt(const Mat& src,OutputArrayOfArrays mv);
```

### 通道合并

```C++
void merge(InputArrayOfArrays mv,OutputArray dst);
```

## 图像对比度和亮度调整

### 调整策略

$$
g(x) = a*f(x)+b
$$

f(x) 是原图像像素
g(x) 是输出图像像素
a 是增益，用于调节对比度
b 是偏置，用来调节图像的亮度

### 代码

```C++
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;

static void on_ContrastAndBright(int, void*);

int g_nContrastValue;
int g_nBrightValue;
Mat g_srcImage, g_dstImage;

int main()
{
	g_srcImage = imread("lena.png",1);
	g_dstImage = Mat::zeros(g_srcImage.size(), g_srcImage.type());
	g_nBrightValue = 80;
	g_nContrastValue = 80;
	namedWindow("result",1);
	createTrackbar("对比度：","result",&g_nContrastValue,300,on_ContrastAndBright);
	createTrackbar("亮度：","result",&g_nBrightValue,200,on_ContrastAndBright);
	on_ContrastAndBright(g_nBrightValue,0);
	on_ContrastAndBright(g_nContrastValue, 0);
	while (waitKey(1)!=0)
	{

	}
	return 0;
}

static void on_ContrastAndBright(int, void*)
{
	namedWindow("origin", 1);
	for (int y = 0; y < g_srcImage.rows; y++)
	{
		for (int x = 0; x < g_srcImage.cols; x++)
		{
			for (int c = 0; c < 3; c++)
			{
				g_dstImage.at<Vec3b>(y, x)[c] =
					saturate_cast<uchar>((g_nContrastValue * 0.01)*(g_srcImage.at<Vec3b>(y, x)[c]) +
						g_nBrightValue);
			}
		}
	}
	//进行优化，使用convertTo函数
	//g_srcImage.convertTo(g_dstImage, -1, g_nContrastValue * 0.01, g_nBrightValue);

	imshow("origin", g_srcImage);
	imshow("result", g_dstImage);
}

```

## 离散傅里叶变换

### dft 函数

dft 函数的作用是对一维或二维浮点数数组进行正向或反向傅里叶变换。

`void dft(InputArray src,OutputArray dst,int flag =0,int nonzeroRows = 0) `

其中 flag 代表转换的标识符

常见的有：

- DFT_INVERSE 逆变换
- DFT_SCALE 缩放，通常和 INVERSE 一起使用
- DFT_ROWS 对输入矩阵每行进行正或反变换

nonzeroRows 如果设置为非零数，那么函数会认为只有输入矩阵的第一个非零行包含非零值，会对其他行进行更加高效的处理。

返回 DFT 最优尺寸大小：
为了提高 DFT 的运行速度，需要扩充图像，具体扩充多少就可以由这个函数计算得到。
`int getOptimalDFTSize(int vecsize)`

扩充图像边界：

```C++
void copyMakeBorder(InputArray src,OutputArray dst,int top,int bottom,int left,int right,int boardtype,const Scalar& value = Scalar());
```

top bottom left right 代表在源图像上下左右各扩展多少像素，boardtype 代表边界类型，Scalar 代表边界值

计算二维矢量幅值：

```C++
void magnitude(InputArray x,InputArray y,OutputArray magnitude)
```

矩阵归一化：

```C++
void normalize(InputArray src,OutputArray dst,double alpha=1,double beta=0,int norm_type=NORM_L2,int dtype=-1,InputArray mask = noArray())
```

alpha 为最大值，beta 为最小值

## 输入输出XML和YAML文件

### 打开XML YAML文件

``` C++

FileStorage fs("a.xml",FileStorage::WRITE);

FileStorage fs;
fs.open("a.xml",FileStorage::WRITE);

```

### 进行读写操作

``` C++

fs << "iterationNr" << 100 ;

int itNr;
fs["iterationNr"] >> itNr;
itNr = (int) fs["iterationNr"]

```

### 文件关闭

`fs.release();`

