---
title: NVIC理解
date: 2019-11-10 10:28:39
categories: STM32底层
tags:
---

## STM32 NVIC 中断优先级管理

STM32有84个中断，16个内核中断，68个可屏蔽中断(但是F103中只有60个) 
 STM32采用中断分组的方法来确定中断的优先级，IPR寄存器组由15个32bit的寄存器组成，每个可屏蔽中断占用8bit，这样就可以表示60个可屏蔽中断·，而每个可屏蔽中断占用的8bit并没有完全使用，而是只用了高4位。这4位又分为抢占优先级和响应优先级。抢占优先级在前，相应优先级在后。而这两个优先级各占几个位又要由SCB->AIRCR寄存器来定义。
比如组设置为3，那么此时所有的60个中断中，每个中断的中断优先级寄存器的高4位中的最高3位是抢占优先级，低一位是相应优先级。每个中断可以设置抢占优先级为0~7，相应优先级为0或1.抢占优先级的级别高于相应优先级。而数值越小所代表的优先级越高。

*这里需要注意两点：*

1. 如果两个中断的抢占优先级和相应优先级都是一样的话，则看哪个中断先发生就先执行；
2. 高优先级的抢占优先级可以打断正在进行的低抢占优先级中断的。而抢占优先级相同的中断，高优先级的相应优先级不可以打断地相应优先级的中断。

## 设置

设置中断优先级顺序：

1. 系统运行开始时设置中断分NVIC_PriorityGroupConfig（）；
2. 设置用到的中断的中断优先级别。调用NVIC_Init（）；
