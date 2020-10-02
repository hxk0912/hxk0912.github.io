---
title: ADC 使用
date: 2020-10-02 13:03:23
categories: STM32底层
tags:
---

## cubeMX配置

1. 模式选择
   - 通常为独立模式，如果要使用多重模式打开多个adc后选择多重模式
2. 设置
   - 分频通常为4分频或6分频
   - 分辨率一般为12bits
   - Data Alignment 为右对齐
   - Scan Conversion Mode 多个通道时要使能
   - 连续转换使能
   - DMA连续请求使能
3. 规则通道设置
   - 设置转换通道数
   - 转换触发源（通常为软件触发）
   - Rank为转换顺序，选择通道和转换周期
4. DMA配置
   - 新建DMA请求，模式选择循环发送（circular）
   - Data Width选择Half Word（程序里缓冲区就要写uint16_t）
5. 中断配置
   - 打开中断

## MDK代码

``` C

__IO uint16_t ADC_Value[2] = {0}; // 建议使用__IO关键字，对应于Half Word

//不是所有芯片都支持校准
//HAL_ADCEx_Calibration_Start(&hadc1);//校准
HAL_ADC_Start_DMA(&hadc1, (uint32_t * )ADC_Buf, 2);

```
