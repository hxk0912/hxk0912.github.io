---
title: pandas基础
date: 2020-07-10 12:32:32
categories: pandas
tags:
---

## pandas数据类型

### Series

Series 是 Pandas 中最基本的一维数组形式。其可以储存整数、浮点数、字符串等类型的数据

基本结构`pandas.Series(data=None, index=None)`

其中，data 可以是字典，或者NumPy 里的 ndarray 对象等。index 是数据索引，索引是 Pandas 数据结构中的一大特性，它主要的功能是帮助我们更快速地定位数据。

#### 创建一个series

``` python
import pandas as pd

s= pd.Series({'a':10,'b':20,'c':30})

```

如果series是从array转换过去，那么索引默认是array的下标 

### DataFrame

DataFrame 是 Pandas 中最为常见、最重要且使用频率最高的数据结构。DataFrame 和平常的电子表格或 SQL 表结构相似。你可以把 DataFrame 看成是 Series 的扩展类型，它仿佛是由多个 Series 拼合而成。它和 Series 的直观区别在于，数据不但具有行索引，且具有列索引。

<img width='400px' src="https://doc.shiyanlou.com/courses/uid214893-20190531-1559284057250">

基本结构：`pandas.DataFrame(data=None, index=None, columns=None)`

就是在Series的基础上增加了columns列索引

#### 创建一个DataFrame

``` python
df = pd.DataFrame({'one': pd.Series([1, 2, 3]),
                   'two': pd.Series([4, 5, 6])})

df = pd.DataFrame({'one': [1, 2, 3],
                   'two': [4, 5, 6]})

df = pd.DataFrame([{'one': 1, 'two': 4},
                   {'one': 2, 'two': 5},
                   {'one': 3, 'two': 6}])
```

## 数据读取

### 读取CSV文件

方法：`pandas.read_csv()`

pd.read_ 前缀开始的方法还可以读取各式各样的数据文件，且支持连接数据库

## 基本操作

### 数据预览

有些时候，我们读取的文件很大。如果全部输出预览这些文件，既不美观，又很耗时。还好，Pandas 提供了 `head()` 和 `tail()` 方法，它可以帮助我们只预览一小块数据。

### 统计性描述

Pandas 还提供了统计和描述性方法，方便你从宏观的角度去了解数据集。`describe()` 相当于对数据集进行概览，会输出该数据集每一列数据的计数、最大值、最小值等。

### 与numpy之间转换

`.values` 可以将 DataFrame 转换为 NumPy 数组。

## 数据选择

### 基于索引数字选择

方法：`.iloc()`

该方法可以接受的类型有：

1.  整数。例如：`5`
2.  整数构成的列表或数组。例如：`[1, 2, 3]`
3.  布尔数组。
4.  可返回索引值的函数或参数。

### 基于标签名称选择

方法：`.loc()`

可以接受的类型有：

1.  单个标签。例如：`2` 或 `'a'`，这里的 `2` 指的是标签而不是索引位置。
2.  列表或数组包含的标签。例如：`['A', 'B', 'C']`。
3.  切片对象。例如：`'A':'E'`，注意这里和上面切片的不同支持，首尾都包含在内。
4.  布尔数组。
5.  可返回标签的函数或参数。

## 数据增删

方法：`.drop()`

pandas.DataFrame.drop) 可以直接去掉数据集中指定的列和行。一般在使用时，我们指定 `labels` 标签参数，然后再通过 `axis` 指定按列或按行删除即可。当然，你也可以通过索引参数删除数据，具体查看官方文档。

`df.drop(labels=['a', 'b'], axis=1)`

`DataFrame.drop_duplicates` 则通常用于数据去重，即剔除数据集中的重复值。使用方法非常简单，指定去除重复值规则，以及 `axis` 按列还是按行去除即可。

除此之外，另一个用于数据删减的方法 `DataFrame.dropna` 也十分常用，其主要的用途是删除缺少值，即数据集中空缺的数据列或行。

## 数据填充

### 检测缺失值

Pandas 为了更方便地检测缺失值，将不同类型数据的缺失均采用 `NaN` 标记。这里的 NaN 代表 Not a Number，它仅仅是作为一个标记。例外是，在时间序列里，时间戳的丢失采用 `NaT` 标记。

Pandas 中用于检测缺失值主要用到两个方法，分别是：`isna()` 和 `notna()`，顾名思义就是「是缺失值」和「不是缺失值」。默认会返回布尔值用于判断。

### 填充

方法：`.fillna()` 

用相同的标量值替换NaN

### 插值填充

我们可以通过 `interpolate()` 方法完成线性插值。

对于 interpolate() 支持的插值算法，也就是 method=。下面给出几条选择的建议：

如果你的数据增长速率越来越快，可以选择 method='quadratic'二次插值。

如果数据集呈现出累计分布的样子，推荐选择 method='pchip'。

如果需要填补缺省值，以平滑绘图为目标，推荐选择 method='akima'。

当然，最后提到的 method='akima'，需要你的环境中安装了 Scipy 库。除此之外，method='barycentric' 和 method='pchip' 同样也需要 Scipy 才能使用。

## 数据可视化

方法：`DataFrame.plot()` 

可以绘制多种样式 指定kind=参数即可

`DataFrame.plot(kind='bar')`