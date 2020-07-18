---
title: FreeRTOS多任务基础
date: 2020-07-18 20:07:46
categories: FreeRTOS
tags:
---

## 任务调度机制

<img src="http://m.qpic.cn/psc?/V11NehB63qJi50/xZikVHqhLrt9jsfqm9tF*Ugvmb7pnE4H33M6c631poa0pBgFVpqEvDJxFznq39nh3SXW4gVHJ9J23j8gogzVqg!!/b&bo=wQQrAwAAAAARB90!&rf=viewer_4">

## 任务特性

1. 简单-使用简单，编程简单
2. 没有限制-任务创建数量没有限制
3. 支持抢占-高优先级可以抢占低优先级
4. 优先级-任务支持优先级排序
5. 独立堆栈-任务切换时，需要保存CPU运行环境

## 任务状态

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/xZikVHqhLrt9jsfqm9tF*bpFp8uiuO6QrBdAjQPGLkbvVDQh41q6b9hiATFB2*oThivXZpN3bCj9qArfZDFVog!!/b&bo=XAMxAwAAAAARB10!&rf=viewer_4">

## 任务优先级

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/xZikVHqhLrt9jsfqm9tF*XDOOqSg.UA8y5RdeNIu6p8.dmN.sxXMGjtYgp9PkJAMRD3jv54cW2maDWE1v4q6XQ!!/b&bo=CgPoAgAAAAARB9M!&rf=viewer_4">

## 任务实现

### 死循环

每个任务都是一个死循环，可以理解为每个任务都有一个main函数入口

### 任务实体

具体的任务功能，任务调度

### 任务退出

任务实现函数没有返回值


## 任务挂起恢复实验

功能：按键检测，按下按键挂起任务，松开按键恢复任务

