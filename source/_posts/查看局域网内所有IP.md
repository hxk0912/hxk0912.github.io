---
title: 查看局域网内所有IP
date: 2020-05-19 08:16:04
categories: Linux
tags:
---

## 步骤

1. 打开命令行
2. 输入`for /L %i IN (1,1,254) DO ping -w 1 -n 1 192.168.10.%i （这里自行修改）`
3. 输入`arp -a`就可以查看了