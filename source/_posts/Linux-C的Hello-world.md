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

![alt](http://m.qpic.cn/psc?/V11NehB63qJi50/9vuGDcz9AP*EJeMjs9i.nksT9*NgdOuMJwLtrHJcAl3LnoOcZ1WXWvhEOaoZMTdzZdM.NrBQVK7vVmpBB1uZKng0RmUnW7lieQbdS9*NVVM!/b&bo=4ALwAQAAAAADBzE!&rf=viewer_4)

附： GCC命令

**gcc [选项] [文件名]**

-c 只编译不链接为可执行文件，编译器将输入的 .c文件编译为 .o的目标文件。
-o：：<输出文件名 > 用来指定编译结束以后的输出文件名，如果使用这个选项的话 GCC认编译出来的可执行文件名字为 a.out。
-g 添加调试信息，如果要使用调试工具 (如 GDB)的话就必须加入此选项，此选项指示编译的时候生成调试所需的符号信息。
-O 对程序进行优化编译，如果使 用此选项的话整个源代码在编译、链接的的时候都会进行优化，这样产生的可执行文件执行效率就高。
-O2 比 -O更幅度更大的优化，生成的可执行效率更高，但是整个编译过程会很慢。