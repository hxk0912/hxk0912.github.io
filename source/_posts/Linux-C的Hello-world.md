---
title: Linux-C的Hello-world
date: 2020-01-29 14:32:44
categories: Linux
tags:
---

## 编写代码

```  C
#include <stdio.h>

int main()
{
    printf("Hello,world!");
}

```

## 编译代码

利用gcc编译器

`gcc main.c -o main //指定生成的可执行文件名字为main`

## 运行代码

如果不指定名字，默认编译生成为a.out
运行命令为`./a.out`

如果指定了名字，如上。
运行命令为`./main`

## 运行效果

