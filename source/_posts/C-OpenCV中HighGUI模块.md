---
title: C++ OpenCV中HighGUI模块
date: 2020-10-28 11:07:59
categories: OpenCV
tags:
---

## 图像的载入

`imread("1.jpg",flag);`

其中第二个参数表示读取图像的颜色类型

当flag<0时，返回包含Alpha通道的加载图像，flag>0时，返回3通道的彩色图像，flag=0时，返回灰度图像。

## 创建窗口

namedWindow("img",flags)

flags默认为，WINDOW_AUTOSIZE 自动分配窗口大小，还可以填WINDOW_NORMAL用户可以手动更改大小，WINDOW_OPENGL，窗口支持OPENGL。

## 滑动条创建与使用（Trackbar）

``` C
int createTrackbar(conststring& trackbarname,conststring& winname,int* value,int count,TrackbarCallback onChange=0,void* userdata =0); 
```

其中，winname代表窗口名（Trackbar必须依赖于窗口创建），value代表Trackbar的返回值，count代表最大值，onChange代表回调函数，而且函数原型必须是`void XXXX(int,void*);`int代表是滑块当前位置，每次滑块位置改变的时候都会调用回调函数。

### 获取当前轨迹条的位置

```C++

int getTrackbarPos(conststring& trackbarname,conststring& winname);

```

## 鼠标操作

``` C
void setMouseCallback(conststring& winname,Mousecallback onMouse,void* userdata=0);
```

第一个参数是窗口的名字，第二个参数是，指定每次鼠标事件发生时引用的回调函数，第三个参数是用户定义的传递给回调函数的参数。

回调函数格式：
```C++
void Foo(int event,int x,int y,int flags,void* params)
```


