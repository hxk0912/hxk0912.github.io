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

简单总结：
[目标]：[依赖]
    [命令]

### 执行过程

1. make命令会在当前目录下查找以 Makefile(makefile其实也可以 )命名的文件。
2. 当找到 Makefile文件以后就会按照 Makefile中定义的规则去编译生成最终的目标文件。
3. 当发现目标文件不存在，或者目标所依赖的文件比目标文件新 (也就是最后修改时间比目标文件晚 )的话就会执行后面的命令来更新目标。

## 伪目标

比如我们文件夹中有了一个名叫 clean的文件，那么每次都是最新的文件，就不会正常执行我们定义的命令了。

为了解决这种问题，Makefile使用”.PHONY”前缀来区分目标代号和目标文件，并且这种目标代号被称为”伪目标”。

也就是说如果不需要生成文件，就把目标定义为伪目标就好

```makefile

.PHONY:clean
clean: 
    rm -f *.o
```

## 默认规则

当找不到xxx. o文件时，会查找目录下的同名xxx.c文件进行编译。

比如我需要a.o而且我文件夹内有a.c，那么我就可以省略编译a.c编程a.o的代码。

``` makefile
#下面两行根据默认规则可以省略了
#a.o:a.c
#    gcc -c a.c
```

## Makefile变量

makefile中变量就类似于C语言中的宏定义

``` makefile
#   makefile 变量的使用
macro = main.o a.o b.o
main:$(macro)
    gcc -o main $(macro)
```

### 赋值符

* =  延时赋值，该变量只有在调用的时候，才会被赋值,变量赋值后会一直使用最新的变量值
* := 直接赋值，与延时赋值相反，使用直接赋值的话，变量的值定义时就已经确定了,变量赋值后只会使用最初定义的变量值
* ?= 若变量的值为空，则进行赋值，通常用于设置默认值。,变量赋值时如果前面有定义就使用前面的值，没有定义就用这个值
* += 追加赋值，可以往变量后面增加新的内容,就像字符串的拼接

### 改造默认规则

在默认生成规则中，并不包括头文件的依赖，我们如果想加入头文件的依赖，可以使用"%"的通配符和变量来改造规则

``` makefile
HEAD = a.h

%.o:%.c $(HEAD)
```

%.o代表了所有以.o结尾的文件，所以这个命令就是相当于在默认规则的基础上加上了头文件的依赖。

### 改造链接规则

我们可以利用变量和自动化变量来使得链接过程更加的简洁和通用

``` makefile
#定义变量
TARGET = main
CC = gcc
HEAD = a.h
OBJS = main.o main.h

#目标文件
$(TARGET): $(OBJS)
 $(CC) -o $@ $^

#*.o文件的生成规则
%.o: %.c $(HEAD)
$(CC) -c -o $@ $<

#伪目标
.PHONY: clean
clean:
rm -f \*.o main
```

经过改造后可以明显看出来整个过程变得很简洁，只需要修改变量就可以改变整个生成过程，使得整个过程很通用。

### 自动化变量

符号|意义
-|-
$@|匹配目标文件
\$%|与$@类似，但$%仅匹配”库”类型的目标文件
$<|依赖中的第一个目标文件
$^|所有的依赖目标，如果依赖中有重复的，只保留一份
$+|所有的依赖目标，即使依赖中有重复的也原样保留
$?|所有比目标要新的依赖目标

## 使用分支

``` makefile
ifeq(arg1, arg2)
分支1
else
分支2
endif
```

常用于修改编译器

``` makefile
#根据输入的ARCH变量来选择编译器
#ARCH=x86，使用gcc
#ARCH=arm，使用arm-gcc
ifeq ($(ARCH),x86)
CC = gcc
else
CC = arm-linux-gnueabihf-gcc
endif
```

同时可以在make命令时修改ARCH来更改编译器`make ARCH=arm`,这样就是使用arm-gcc

## 使用函数

在实际工程中，头文件、源文件可能会放在二级目录，编译生成的*.o或可执行文件也放到专门的编译输出目录方便整理，实现这些功能就要使用Makefile函数。

### 函数使用格式

``` makefile
$(函数名 参数)
#或者使用花括号
${函数名 参数}
```

### 常用函数

#### notdir函数

notdir函数用于去除文件路径中的目录部分。它的格式如下：`$(notdir 文件名)`

例如输入参数”./sources/a.c”，函数执行后 的输出为”a.c”，也就是说它会把输入中的”./sources/”路径部分去掉，保留文件名。

#### wildcard函数

wildcard函数用于获取文件列表，并使用空格分隔开。它的格式如下：`$(wildcard 匹配规则)`

例如函数调用”$(wildcard *.c)”，函数执行后会把当前目录的所 有c文件列出。

#### patsubst函数

patsubst函数功能为模式字符串替换。它的格式如下：`$(patsubst 匹配规则, 替换规则, 输入的字符串)`
当输入的字符串符合匹配规则，那么使用替换规则来替换字符串，当匹配规则中有”%”号时，替换规则也可以例程”%”号来提取”%”匹配的内容加入到最后替换的字符串中。

### 多级结构工程的Makefile

``` makefile
#定义变量
#ARCH默认为x86，使用gcc编译器，
#否则使用arm编译器
ARCH ?= x86
TARGET = hello_main

#存放中间文件的路径
BUILD_DIR = build_$(ARCH)
#存放源文件的文件夹
SRC_DIR = sources
#存放头文件的文件夹
INC_DIR = includes .

#源文件
SRCS = $(wildcard $(SRC_DIR)/*.c)
#目标文件（*.o）
OBJS = $(patsubst %.c, $(BUILD_DIR)/%.o, $(notdir $(SRCS)))
#头文件
DEPS = $(wildcard $(INC_DIR)/*.h)

#指定头文件的路径
CFLAGS = $(patsubst %, -I%, $(INC_DIR))

#根据输入的ARCH变量来选择编译器
#ARCH=x86，使用gcc
#ARCH=arm，使用arm-gcc
ifeq ($(ARCH),x86)
CC = gcc
else
CC = arm-linux-gnueabihf-gcc
endif

#目标文件
$(BUILD_DIR)/$(TARGET): $(OBJS)
$(CC) -o $@ $^ $(CFLAGS)

#*.o文件的生成规则
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c $(DEPS)
#创建一个编译目录，用于存放过程文件
#命令前带"@",表示不在终端上输出
@mkdir -p $(BUILD_DIR)
$(CC) -c -o $@ $< $(CFLAGS)

#伪目标
.PHONY: clean cleanall
#按架构删除
clean:
rm -rf $(BUILD_DIR)

#全部删除
cleanall:
rm -rf build_x86 build_arm
```
