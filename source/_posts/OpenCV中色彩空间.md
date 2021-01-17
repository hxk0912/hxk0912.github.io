---
title: OpenCV中色彩空间
date: 2021-01-12 18:53:15
Categories: OpenCV
tags:
---

## RGB颜色空间

这是一种加性的颜色空间，颜色是由R，G,B三种颜色混合而成

![](http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5ugj5uxKDUO1vVh*TKAHYvssOyVJD.fLrLOhLOkkEnmJg2qtQ9dK3kSlFEOkS76g0fm70wc7S75KJOo0wOWjTak!/b&bo=XQI.AQAAAAADB0I!&rf=viewer_4)

由上图可以看出因为三种颜色的分布不均匀性，导致他们受光照变化的影响不同，导致同一颜色在不同照度下会产生明显差异

所以RGB颜色空间的缺点是：明显的感知不均匀、色度和强度信息混合在一起

## LAB颜色空间

LAB色彩空间有三个组成部分

1. L -亮度(强度)
2. A -color成分，从绿色到品红色
3. B -color分量从蓝色到黄色不等

![](http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5qX8hFR4qiJzV9Xfy*rvOi9Ogt4fk2SYDXGZt42XEwbsJRh2J3*dsLwgpWgAh2QXfFfwm16RZACPHRuSpXAtp2A!/b&bo=XQI.AQAAAAADB0I!&rf=viewer_4)

LAB色彩空间有以下特点：

- 感知上一致的颜色空间，近似于我们感知颜色的方式
- 独立于设备(捕获或显示)
- 在adobe photoshop中广泛使用
- 与RGB颜色空间相关的是一个复杂的变换方程

从图中可以清楚地知道，照明的变化主要影响了 L 分量。
包含颜色信息的 A 和 B 组件没有发生大规模更改。

## YCrCb颜色空间

1. Y = 伽玛校正后从 RGB 获得的亮度或亮度分量。
2. Cr = R = Y （红色分量离 Luma 有多远 ）
3. Cb = B = Y （蓝色分量离 Luma 有多远 ）

- 将亮度和色度分量分离到不同的通道中。
- 主要用于压缩 （ Cr 和 Cb 组件 ） 电视传输.
- 设备相关。

![](http://m.qpic.cn/psc?/V11NehB63qJi50/ruAMsa53pVQWN7FLK88i5r1xkMmWwsNKa7xdcqnh.ly26FvBBB.OuXpRV.ysEDYf70*m1S3xwbbWFxvTJmPO.5gdkgDo76eaCFGQfKLdybw!/b&bo=XQI.AQAAAAADB0I!&rf=viewer_4)

从图中可以看出：

- 与 LAB 类似的观测值可用于强度和颜色分量对照明变化的影响。
- 与 LAB 相比，红色和橙色之间的感知差异在户外图像中也较小。
- 白色在所有 3 个组件中都发生了更改。

## HSV颜色空间

1. H = 色调 （ 显光波长 ）.
2. S = 饱和度 （颜色的纯度 / 阴影 ）。
3. V = 值 （ 强度 ）

最好的事情是，它只使用一个通道来描述颜色（H），使得它非常直观地指定颜色。
设备相关。

![](http://m.qpic.cn/psc?/V11NehB63qJi50/45NBuzDIW489QBoVep5mccCSuMdGEdkTJPgw1NOA1o30IaaUOiGw27kV8h9Ch1voyhNBJEyMghw*se9l3n0OL8Eag1g.rv3yELozhdjyqWs!/b&bo=XQI.AQAAAAADF1I!&rf=viewer_4)

- H 组件在两个图像中非常相似，表示即使在照明变化下，颜色信息也完好无损。
- S 组件在两个图像中也非常相似。
- V 组件捕获落在其上的光量，因此由于照明变化而变化。
- 室外图像和室内图像的红色值存在巨大差异。这是因为色调表示为圆，红色位于起始角度。因此，它可能需要 [300， 360] 和再次 [0， 60] 之间的值。