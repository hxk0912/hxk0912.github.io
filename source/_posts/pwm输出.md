---
title: pwm输出
date: 2019-10-25 12:59:41
categories: STM32底层
tags:
---

## 原理

见定时器理解，四个寄存器：CNT,ARR,PSC,CCR

## cubemx配置

1. 选择定时器后选择所需PWM产生模式。

2. 选择内部产生的时钟源。

3. 配置Prescaler项，也就是PSC寄存器,为x-1。

4. 配置Counter Period项，也就是ARR寄存器。

5. 配置各个通道的Pulse项，也就是CCR寄存器。

## MDK函数

在MDK中要开启pwm输出，需要调用函数：

``` C
HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_1);//开启TIM1 CH1的PWM输出
```

**需要注意的是，如果想要同时开启多个通道的时候，不能使用 | 符号来连接多个通道，应该一次写一个通道的开启。**

