---
title: FSMC/FMC接口理解
date: 2019-12-10 20:49:49
categories: STM32底层
tags:
---

## FSMC概念

FSMC，即灵活的静态存储控制器，能够与同步或异步存储器和 16 位 PC 存储器卡连接，STM32 的 FSMC 接口支持包括 SRAM、NAND FLASH、NOR FLASH 和 PSRAM 等储器。

STM32F429的FMC将外部存储器划分为6个固定大小为256M字节的存储区域。如下图所示：

![alt](http://m.qpic.cn/psb?/V11NehB63qJi50/egon5y1iXaz548frLkt5ljvKcoCx3e1IzDUoVUBnDIM!/b/dL8AAAAAAAAA&bo=fQKIAwAAAAADB9Y!&rf=viewer_4)

从上图可以看出，FMC总共管理1.5GB空间，拥有6个存储块（Bank）。
STM32F429的FMC存储块1（Bank1）被分为4个区，每个区管理64M字节空间，每个区都有独立的寄存器对所连接的存储器进行配置。Bank1的256M字节空间由28根地址线（HADDR[27:0]）寻址。这里HADDR是内部AHB地址总线，其中HADDR[25:0]来自外部存储器地址FMC_A[25:0]，而HADDR[26:27]对4个区进行寻址。
HADDR[25:0]位包含外部存储器的地址，由于HADDR为字节地址，而存储器按字寻址，所以，根据存储器数据宽度的不同，实际上向存储器发送的地址也有所不同。
因此，FMC内部HADDR与存储器寻址地址的实际对应关系就是：

当接的是32位宽度存储器的时候：HADDR[25:2] FMC_A [23:0]。

当接的是16位宽度存储器的时候：HADDR[25:1] FMC_A [24:0]。

当接的是8位宽度存储器的时候：HADDR[25:0] FMC_A [25:0]。

**不论外部接8位/16位/32位宽设备，FMC_A[0]永远接在外部设备地址A[0]。**
