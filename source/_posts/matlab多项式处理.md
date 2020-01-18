---
title: matlab多项式处理
date: 2020-01-18 19:33:09
categories: matlab
tags:
---

## 多项式计算

### 多项式表示

直接建立多项式向量就可以，将系数作为元素即可。
注意：

1. 从高到低[a<sub>n</sub>,···,a<sub>0</sub>]
2. 长度为多项式最高次加1
3. 没有的项系数用0补足

### 多项式的四则运算

加减即为向量加减

#### 多项式除法

[Q,r]=deconv(P1,P2)

Q返回商式，r返回余式，两者仍是多项式系数向量

#### 多项式乘法

conv(P1,P2)

#### 多项式的求导

polyder()

调用格式：

1. p=polyder(P)  求多项式P的导函数
2. p=polyder(P，Q)  求多项式P·Q的导函数
3. [p,q]=polyder(P，Q)  求多项式P/Q的导函数，分子存入p，分母存入q

#### 多项式的求值

polyval(p,x):代数多项式求值
polyvalm(p,x)：矩阵多项式求值
**区别：polyvalm要求为方阵，乘法是\*,而polyval是.\*。**

#### 多项式求根

