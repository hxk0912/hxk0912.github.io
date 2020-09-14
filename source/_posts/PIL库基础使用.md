---
title: PIL库基础使用
date: 2020-09-14 20:44:40
categories: PIL
tags:
---


## 基础使用

Image 是 PIL库中一个代表图像的类

``` python

from PIL import Image

img = Image.open() # 打开图片

# 图片融合、合成

PIL.Image.alpha_composite(im1,im2)

PIL.Image.blend(im1,im2,alpha)

PIL.Image.composite(im1,im2,mask)

Image.convert(mode=None,matrix=None,dither=None,palette=0,color=256)

bands = im.getbands() #显示该图像的所有通道

bboxs = im.getbbox() # 返回一个像素坐标

```