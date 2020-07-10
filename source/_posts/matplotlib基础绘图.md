---
title: matplotlib基础绘图
date: 2020-07-07 17:08:59
categories: matplotlib
tags:
---

## 折线图

pyplot中的plot对应于matlab中plot函数，用于绘制折线图

``` python
from matplotlib import pyplot as plt

x = range(1, 6)

y = [1, 2, 3, 4, 5]

plt.plot(x, y)

plt.show()

```

### 更改图片大小和清晰度

``` python
from matplotlib import pyplot as plt

"""
figure 用于设置图片，figsize用于设置图片大小，dpi用于设置清晰程度
"""

fig = plt.figure(figsize=(20,8),dpi=80)

x = range(1, 6)

y = [1, 2, 3, 4, 5]

plt.plot(x, y)

# plt.savefig("./fig1.svg") 可以保存图片

plt.show()
```

### 设置坐标轴

``` python
_x_ticks_lables = [i for i in range(0,7)]

plt.xticks(_x_ticks_lables)

plt.yticks(range(min(y),max(y)+1))
```

### 字符串坐标轴设置

``` python
_x_ticks_lables = ["10h{}min".format(i) for i in range(1,6)]

plt.xticks(x,_x_ticks_lables,rotation=45)#　rotation 用于旋转
```

### 设置中文标题

``` python
# 在前面加入以下两句
from matplotlib.font_manager import FontProperties

font = FontProperties(fname=r"C:\Windows\Fonts\msyh.ttc",size=15)# 微软雅黑，15号大小

# x轴标签
plt.xlabel("时间",FontProperties=font)

# 标题
plt.title("画图",FontProperties=font)
```

### 网格

``` python
plt.grid(alpha=0.4) # alpha用于设置透明度
```

### 同一个图中绘制多条线并为每条线添加图例

``` python
# plot引用时添加label
plt.plot(x, y, label="1")
plt.plot(x, z, label="2")
# 使用图例
plt.legend(prop=font)# 注意这里使用prop接收font字体
```

### 线条颜色样式设置

`plt.plot(x, y, 'r--')`

## 散点图

``` python
plt.scatter(x, y, label="1") # 只有调用的函数不同，其他与plot相同
```

## 条形图

``` python
_bar_width=0.2

x_y=[i for i in x]
x_z=[i+_bar_width for i in x] # 注意这里x轴坐标的平移

plt.bar(x_y, y, label="y",width=_bar_width)
plt.bar(x_z, z,label="z",width=_bar_width)
```

## 直方图

``` python
"""
x为原始数据，5是组数
直方图适合直接用于处理原始数据
density表示归一化，True表示频率分布直方图
"""
plt.hist(x,5,density=True)
```