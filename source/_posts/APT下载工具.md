---
title: APT下载工具
date: 2020-01-28 19:45:27
categories: Linux
tags:
---

## 功能

APT下载工具可以实现软件自动下载、配置、安装二进制或者源码的功能。

## 安装源

APT采用的 C/S模式，也就是客户端 /服务器模式，我们的 PC机作为客户端，当需要下载软件的时候就向服务器请求，因此我们需要知道服务器的地址，也叫做安装源或者更新源。

设置在“软件和更新”处，选择中国的服务器

## 常用命令

1. 更新本地数据库

sudo apt-get update

这个命令会访问源地址，并且获取软件列表并保存在本电脑上

2. 检查依赖关系

sudo apt-get check

有时候本地某些软件可能存在依赖关系，所谓依赖关系就是A软件依赖于B软件。

3. 软件安装

sudo apt-get install package-name

4. 软件更新

sudo apt-get upgrade package-name

5. 卸载软件

sudo apt-get remove package-name

