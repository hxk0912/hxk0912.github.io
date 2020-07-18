---
title: FreeRTOS系统引入
date: 2020-07-16 19:40:27
categories: FreeRTOS
tags:
---

## 前后台系统（裸机开发）

所有操作系统的入口都是中断

中断的目的是提高效率，而且中断用于处理紧急事件，处理时间要尽量短

### 前后台系统的问题

1. 实时性不能够得到保证
2. CPU利用率不高
3. 代码结构复杂

## RTOS引入

### 什么是RTOS

实时操作系统，分为硬实时和软实时，硬实时要求规定时间内必须完成操作，不允许超时，软实时对于超时的处理没有那么严格。

实时性：在固定时间内对事件进行响应，但是实时不代表快。
操作系统：一种系统软件，提供任务管理和协调的控制功能
终端：运行特定的嵌入式硬件，功能可裁剪，代码可移植

**RTOS的核心就是任务调度**

### 多个工作流

简单来说就是RTOS使得一个物理CPU工作起来像多个CPU一样

### RTOS工作原理

<img src = "http://m.qpic.cn/psc?/V11NehB63qJi50/xZikVHqhLrt9jsfqm9tF*RSaHTTKy5ObyiSHy9NObiI*rMxAZQCjEeECA4wFpowrKhLhl.ywwUBL2X9C7LPfCQ!!/b&bo=TQQ*AwAAAAARB0U!&rf=viewer_4">

关键点在于任务间的切换，比如在中断后可以切换任务这点与裸机编程有很大差别。

### RTOS核心组件

资源访问控制：信号量、互斥锁、临界区
消息通信：消息队列、事件标志
存储管理：存储块

## FreeRTOS点灯

### cubemx配置

1. 初始化所需引脚
2. SYS选项中Timebase Sourse修改为除Systick外其他一个定时器
3. 配置时钟树
4. 启用FreeRTOS，新建一个Task

### MDK代码

在FreeRTOS.c中 LED_Task下循环中添加

``` C
    
HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin,GPIO_PIN_SET);

osDelay(1000);

HAL_GPIO_WritePin(LED1_GPIO_Port,LED1_Pin,GPIO_PIN_RESET);

osDelay(1000);
```

### 实现效果

灯一秒亮一秒灭不断循环