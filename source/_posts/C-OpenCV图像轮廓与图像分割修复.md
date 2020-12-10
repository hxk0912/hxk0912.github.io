---
title: C OpenCV图像轮廓与图像分割修复
date: 2020-11-21 09:28:49
categories: OpenCV
tags:
---

## 查找并绘制轮廓

### 寻找轮廓

``` C++
void findContours(InputOutputArray src,OutputArrayOfArrays contours,OutputArray hierarchy,int mode,int method,Point offset=Point)
```

其中，输入需要使用二进制图像，检测到的轮廓存储在contours,hierarchy为可选的输出向量。

第四个参数mode为轮廓检索模式，
标识符|含义
-|-
RETR_EXTERNAL|只检测最外层轮廓
RETR_LIST|提取所有轮廓，并且放置在list中，检测的轮廓不建立等级关系
RETR_CCOMP|提取所有轮廓，并且将其组织为双层结构，顶层为连通域的外围便捷，次层为孔的内层边界。
RETE_TREE|提取所有轮廓，并重新建立网状的轮廓结构

第五个参数method为获取轮廓的近似办法。

标识符|含义
-|-
CHAIN_APPROX_NONE|获取轮廓的每个像素
CHAIN_APPROX_SIMPLE|压缩元素，只保留某一方向的终点坐标，例如一个矩形轮廓只需要4个点来保存轮廓信息

### 绘制轮廓

``` C++
void drawContours(InputOutputArray image,InputArrayOfArrays contours,int contoursIdx,const Scalar& color,int thickness=1,int lineType=8,InputArray hierarchy=noArray,int maxLevel=INT_MAX,Point offset=Point())
```

第三个参数Idx为指示绘制哪一个轮廓

thickness为线的粗细程度。

lineType为线型，默认为8连通线型。

