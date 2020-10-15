---
title: python爬虫实践
date: 2020-09-14 21:12:34
categories: 爬虫
tags:
---

## 爬虫初识

### 什么是爬虫

网络爬虫，是一种按照一定规则，自动抓取互联网信息的程序或者脚本。由于互联网数据的多样性和资源的有限性，根据用户需求定向抓取相关网页并分析已成为如今主流的爬去策略。

### 爬虫的本质

模拟浏览器打开网页，获取网页中我们想要的那部分数据。

## 基本流程

### 准备工作

通过浏览器查看分析目标网页，学习编程基础规范。

### 获取数据

通过HTTP库向目标站点发起请求，请求可以包含额外的header等信息，如果服务器能正常响应，会得到一个Response，便是索要获取的页面内容。

### 解析内容

得到的内容可能是HTML、JSON等格式，可以用页面解析库、正则表达式等进行解析。

### 保存数据

保存形式多样，可以存为文本，也可以保存到数据库，或者保存特定格式的文件。

## URL初步概念

整个过程大致会产生以下步骤：

本地客户端->请求->服务器

本地浏览器<-文件数据<-服务器

本地浏览器进行解析文件数据并且展现

URL（Uniform Resource Locator）统一资源定位符。就是我们给浏览器输入的地址。

URL基本格式：`protocol:// hostname[:port] / path / [;parameters][?query]#fragment`

基本上是由三部分组成：
1. 协议（HTTP,FTP）
2. 主机的IP地址
3. 请求主机资源的具体地址

其中第一部分和第二部分用://分割,第二部分和第三部分用/分割

## urllib库使用

### urllib3

``` python

def load_page(url):
    http = urllib3.PoolManager()
    header = {'User-Agent': 'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0;'}
    r = http.request('GET', url, headers=header)
    return r.data

```

这里的header就是User-Agent头，用于伪装成浏览器请求。返回值就是读取到的网页，但是不是string类型，要使用str()转为字符串类型。
