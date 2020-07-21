---
title: FreeRTOS多任务调度
date: 2020-07-20 15:00:22
categories: FreeRTOS
tags:
---

## 任务控制块

FreeRTOS的每个任务都有一些属性需要存储，把这些属性集合到一起用一个结构体表示，这个结构体就是任务控制块(TCB_t)

## 任务操作业务流程

### 任务动态创建业务流程

1. 分配任务控制内存空间，分配任务堆栈空间
2. 初始化任务控制块，初始化任务堆栈
3. 添加任务到就绪列表中

### 任务删除业务流程

1. 从就绪表中删除
2. 从事件表中删除
3. 释放任务控制块，释放任务堆栈内存
4. 开始任务调度

### 任务挂起业务流程

1. 从就绪表中删除
2. 从事件表中删除
3. 添加任务到挂起列表中
4. 开始任务调度

### 任务恢复业务流程

1. 从挂起表中删除
2. 添加任务到就绪列表中
3. 开始任务调度

## 常用任务API

API名称|API说明
-|-
xTaskCreate|动态创建任务
xTaskCreateStaic|静态创建任务
vTaskDelete|删除任务
vTaskSuspend|挂起任务
vTaskResume|恢复任务
xTaskResumeFromISR|在中断中恢复任务
uxTaskPriorityGet|获取任务优先级
vTaskPrioritySet|设置任务优先级
vTaskDelay|延时任务

## 多任务调度基础

### Sysyick的重要性

Systick是调度器的核心

操作系统在Systick中进行上下文切换

