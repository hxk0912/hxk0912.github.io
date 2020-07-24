---
title: FreeRTOS消息队列
date: 2020-07-22 10:42:14
categories: FreeRTOS
tags:
---

## 消息队列概念

### 消息队列的应用

消息队列(queue),可以在任务与任务间、中断和任务间传递消息。

实现任务接收来自其他任务或中断的不固定长度的消息。

简单来说，从中断或者任务接收消息，存储于队列中，然后再传递给另一个任务或者中断。

## 消息队列的使用

### API

- xQueueCreate()：创建一个消息队列，返回消息队列句柄
- xQueueSend():在任务中往队列传入消息
- xQueueSendFromISR()：在中断中往队列中传入消息
- xQueueReceive():在任务中读取消息队列消息

## 消息队列实现原理

### 消息队列控制块

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5mMBdegCsGBV6xY.D0lPyL0fAmLwzWrz5Xvy7LoXPd3WEH.0m14wv*PJ5zii7HY.XHQYJJYAwBaAwC74Wezo7Sg!/b&bo=CQIhAgAAAAABBwg!&rf=viewer_4">

### 消息队列创建

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5vslOGqAUPE4xexOOyvJTUOCfUj91mwvThmKf3YSGErtlMs4drDg5DghlnjVkHoS.4*6O2*LsJSP4hSO.lzbifg!/b&bo=ogIgAgAAAAABB6I!&rf=viewer_44">

### 消息队列在任务中发送

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5q6qWd0k*DTR*nrx2ECyLlZfn6rmw2cN5.jqZtJs1mSNrjkttzy73odYn6.GgpQZALhQiPMUd0TArvG0jmmi0V4!/b&bo=MwODAgAAAAABB5E!&rf=viewer_4">

### 消息队列在任务中发送

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5qDtfqMr1qvQg9TytkYiUFnyJ3f7JzwmoE7aJlx3gZlVBBAwwz9y.hnFxkh0DoKYsfpjKncUw6ZlE2h**SDD3uY!/b&bo=3gEBAgAAAAABB*w!&rf=viewer_4">

### 消息队列在任务中接收

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5kDQnPtheU*1VnDukCjose2hFVPgsVpRA2Wyoe5leFxVXIBk0wsC7tMrpwNc6VMz*OV7F*HX49ZC9dt0wf5hgU4!/b&bo=rgIdAgAAAAABB5M!&rf=viewer_4">

### 消息队列在任务中接收

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5t22ofQEcW4cUDnUmwojB9Csi6Sd7mwtRzBkzUdKu.NcVS7gChqTvrR6zsekgMz.vYOuagS*oP5942YfxFoPyYQ!/b&bo=2QH3AQAAAAABBw4!&rf=viewer_4">
