---
title: Git初始化及仓库操作
date: 2019-11-25 23:17:50
categories: Git
tags:
---

## 基本信息设置

- 设置用户名： git config --global user.name 'xxx'
- 设置用户名邮箱： git config --global user.email 'xxx@xxx.com'

## 初始化一个新的Git仓库

- 首先要创建一个新的文件夹
- 然后在文件夹内git bash，输入git init

## 向仓库添加文件

- git add . （这个是将文件添加到暂存区）
- git commit -m "..." （这个是将文件添加到仓库）

*每一次修改之后使用 git commit -m "..."这个命令修改仓库文件*

