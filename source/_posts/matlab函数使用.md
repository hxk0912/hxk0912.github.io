---
title: matlab函数使用
date: 2020-01-18 13:10:45
categories: matlab
tags:
---

## 函数文件的定义与调用

### 函数文件的基本结构

function 输出形参表=函数名（输入形参表）
注释说明部分
函数体语句

*当有多个形参时，形参之间用逗号分隔，组成形参表。当输出形参多余一个时，应该用方括号括起来，构成一个输出矩阵。*

**注意：函数名与函数文件名是不同概念，但是我们通常情况下，函数文件名通常由函数名再加上扩展名.m组成，函数文件名与函数名也可以不相同。**

- return语句表示结束函数的执行

### 函数的调用

调用格式：[输出实参表]=函数名[输入实参表]

### 匿名函数

基本格式：函数句柄变量=@（匿名函数输入参数）匿名函数表达式

比如：f=@(x,y) x+y
f(3,4)即可得出结果为7

### 函数的递归调用

与C语言用法基本相同。

## 函数参数与变量的作用域

### 函数参数的可调性

*matlab有两个预定义变量用来记录变量的个数，这样对于同一个函数就可以根据输入变量个数不同执行不同的程序。类似于C++中的函数重构。*
nargin ： 输入实参的个数
nargout： 输出实参的个数

### 局部变量和全局变量

与C语言类似。
一般定义的变量都是局部变量

定义全局变量：
global 变量名

**要注意的是无论在哪里使用全局变量都要重新定义一次。**
