---
title: matlab矩阵使用
date: 2020-01-13 14:08:31
categories: matlab
tags:
---

## 矩阵的建立

1. 直接输入法：A=[1,2,3;4,5,6;7,8,9] A=[1 2 3;4 5 6;7 8 9]

2. 利用已经建立好的矩阵拼接创建。 C=[A,B;B,A]

3. 可以用实部矩阵和虚部矩阵构成复数矩阵。 C=A+i*B

4. 冒号表达式 可以产生行向量 
 
    e1:e2:e3 e1初始值 e2步长 e3终止值 t=0:1:5 t= 0 1 2 3 4 5
    
    linspace函数： linspace（a,b,n）a第一个元素 b最后一个元素 n元素个数（不写自动产生100个）同样产生行向量


## 结构矩阵和单元矩阵

### 结构矩阵

`结构矩阵元素.成员名=表达式`

`a(1).x1=10`

### 单元矩阵

单元矩阵直接仿照一般矩阵建立就可以了，各个单元直接就是所需要的数据。**不同的是单元矩阵元素要用大括号括起来。**

## 矩阵元素的引用

- 通过下标来引用

    **A(3,2)** 引用第三行第二列 *如果赋值时，如果给出行下表和列下标大于原矩阵，会自动扩展，未赋值元素会默认为0。*
- 通过序号引用

    在matlab中，矩阵元素按列存储，即首先存储矩阵的第一列元素，然后存储第二列元素，...，一直到最后一列

    序号就是在内存中的排列顺序。

矩阵元素的序号和下标可以利用sub2ind和ind2sub函数实现互相转化。

`D=sub2ind(S,I,J)` S行数和列数组成的向量，通常使用size函数获取，I转换矩阵元素的行下标，J转换据矩阵元素的列下标。

同理，`[I,J]=sub2ind(S,D)`

## 利用冒号表达式获得子矩阵

子矩阵是指由矩阵中的一部分元素构成的矩阵。

- A(i,:) 第i行的全部元素
- A(:,j) 第j列的全部元素
- A(i:i+m,:) 第i~i+m行的全部元素
- A(i:i+m,k+k+m) 第i~i+m行内且在k~k+m列内的全部元素

## 利用空矩阵删除矩阵中的元素

x=[]为一个空矩阵
直接将某些需要删除的位置置为空矩阵就可以完成删除

`A(:,[2,4])=[]`

意味着将第2列和第4列元素删除（并不是置0，而是直接删除）

## 改变矩阵的形状

reshape（A，m，n） 指在矩阵中元素（存储顺序）保持不变的前提下，将矩阵A重新排列成m×n的二维矩阵

A(:) :将矩阵A的每一列元素堆叠起来，成为一个列向量。
