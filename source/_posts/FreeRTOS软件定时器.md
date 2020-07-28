---
title: FreeRTOS软件定时器
date: 2020-07-28 16:08:28
categories: FreeRTOS
tags:
---

## 概念

软件定时器就类似于裸机编程时的溢出中断，计时到达就执行任务，同时还具有选择是否重复进行的功能。

## API

- xTimerCreate 开启软件定时器
- xTimerStart 启动软件定时器
- xTimerReset 重启软件定时器
- pvTimerGetTimerID 获取软件定时器标识
- xTimerChangePeriod 修改软件定时器周期

