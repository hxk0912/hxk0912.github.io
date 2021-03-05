---
title: Django基础
date: 2021-03-05 11:05:36
categories: Django
tags:
---

## WEB程序执行流程

客户端（浏览器）发出HTTP请求，到达服务器端，服务器程序接收解析请求报文，发送给框架程序（Django）。框架程序接受到HTTP请求对象（request）后进行中间层处理，然后生成HTTP相应对象（response），再传回给服务器程序构造返回HTTP响应报文。响应给前端客户端。

