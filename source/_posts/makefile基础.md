---
layout: 
title: makefile基础
date: 2020-01-29 16:39:56
categories: Linux
tags:
---

## 什么是Makefile

我们在进行实际开发的时候工程里面可能有几十上百个文件等待编译，为解决这个问题。用来描述哪些文件需要编译、哪些需要重新编译的文件就叫做Makefile。

## Makefile功能

1. 如果工程没有编译过，那么工程中的所有 .c文件都要被编译并且链接成可执行程序。
2. 如果工程中只有个别 C文件被修改了，那么只 编译这些被修改的 C文件即可。
3. 如果工程的头文件被修改了，那么我们需要编译所有引用这个头文件的 C文件，并且链接成可执行文件。

## Makefile语法

``` Makefile
main:main.o a.o b.o
    gcc -o main main.o a.o b.o
main.o:main.c
    gcc -c main.c
a.o:a.c
    gcc -c a.c
b.o:b.c
    gcc -c b.c

clean:
    rm *.o
    rm main
```

**命令列表中的每条命令必须以TAB键开始，不能使用空格！**

### 执行过程

1. make命令会在当前目录下查找以 Makefile(makefile其实也可以 )命名的文件。
2. 当找到 Makefile文件以后就会按照 Makefile中定义的规则去编译生成最终的目标文件。
3. 当发现目标文件不存在，或者目标所依赖的文件比目标文件新 (也就是最后修改时间比目标文件晚 )的话就会执行后面的命令来更新目标。

### Makefile变量

