---
title: Qt中添加图标
date: 2020-04-14 08:55:41
categories: Qt
tags:
---

## 添加资源文件

1. 将需要添加的资源文件放到工程文件夹下
2. 添加文件中选择Qt Resource 文件
3. 编辑中增加前缀，选择文件。
4. 要添加的文件路径就是“:/前缀名/文件名”

## 添加图标

``` C++
    ui->actionnew->setIcon(QIcon(":/Image/新建.png"));  // 传入参数是QIcon不是指针
    ui->actionedit->setIcon(QIcon(":/Image/修改.png"));
```


