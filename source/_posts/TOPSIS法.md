---
title: TOPSIS法
date: 2020-03-23 22:09:33
categories: 数学建模
tags:
mathjax: true
---

## 处理方式

1. 将原始矩阵正向化
2. 正向矩阵标准化
3. 计算得分并归一化

### 正向化

矩阵正向化就是要将所有的指标类型统一转化为极大型指标。

常见的四种指标：

指标名称|指标特点|例子
-|-|-
极大型指标|越大越好|成绩、GDP增速、企业利润
极小型指标|越小越好|费用、坏品率、污染程度
中间型指标|越接近某个值越好|PH值
区间型指标|落在某个区间最好|体温、营养物量

### 正向化公式

#### 极小型

$$
\hat{x_i}=max(x_i)-x_i
$$

#### 中间型

$$
M=max{|x_i-x_{best}|},\hat{x_i}=1-\frac{|x_i-x_{best}|}{M}
$$

#### 区间型

$$
M=max\{a-min\{x_i\},max\{x_i\}-b\},\\
\hat{x_i}=\begin{cases}
    1-\frac{a-x}{M},x<a\\
    1       ,a\leq x \leq b\\
    1-\frac{x-b}{M},x>b
\end{cases}
$$

### 正向化矩阵标准化

$$
每个元素/\sqrt{其所在列的元素的平方和}
$$

### 计算得分并归一化

<img src="http://m.qpic.cn/psc?/V11NehB63qJi50/xZikVHqhLrt9jsfqm9tF*Qh93FBXh*GEN2j3mjdM3b6eO1sgHCoODTzD6Kuz4kQLBl66VHXe7VE2RGlxBUr7IA!!/b&bo=AQRrAgAAAAARB1w!&rf=viewer_4">

## 示例代码

``` matlab
%正向化处理
function [posit_x] = Positivization(x,type,i)
    if type == 1  %极小型

        posit_x = Min2Max(x);  %调用Min2Max函数来正向化

    elseif type == 2  %中间型

        best = input('请输入最佳的那一个值： ');
        posit_x = Mid2Max(x,best);

    elseif type == 3  %区间型

        a = input('请输入区间的下界： ');
        b = input('请输入区间的上界： '); 
        posit_x = Inter2Max(x,a,b);

    end
end

% 极小型
function [posit_x] = Min2Max(x)
    posit_x = max(x) - x;
end

% 中间型
function [posit_x] = Mid2Max(x,best)
    M = max(abs(x-best));
    posit_x = 1 - abs(x-best) / M;
end

% 区间型
function [posit_x] = Inter2Max(x,a,b)
    r_x = size(x,1);  
    M = max([a-min(x),max(x)-b]);
    posit_x = zeros(r_x,1);   
    for i = 1: r_x
        if x(i) < a
           posit_x(i) = 1-(a-x(i))/M;
        elseif x(i) > b
           posit_x(i) = 1-(x(i)-b)/M;
        else
           posit_x(i) = 1;
        end
    end
end

clear;clc

[n,m] = size(X);
disp(['共有' num2str(n) '个评价对象, ' num2str(m) '个评价指标']) 
Judge = input(['这' num2str(m) '个指标是否需要经过正向化处理，需要请输入1 ，不需要输入0：  ']);

if Judge == 1
    Position = input('请输入需要正向化处理的指标所在的列，例如第2、3、6三列需要处理，那么你需要输入[2,3,6]： '); %[2,3,4]
    disp('请输入需要处理的这些列的指标类型（1：极小型， 2：中间型， 3：区间型） ')
    Type = input('例如：第2列是极小型，第3列是区间型，第6列是中间型，就输入[1,3,2]：  '); 
    % 注意，Position和Type是两个同维度的行向量
    for i = 1 : size(Position,2)  %这里需要对这些列分别处理，因此我们需要知道一共要处理的次数，即循环的次数
        X(:,Position(i)) = Positivization(X(:,Position(i)),Type(i),Position(i));
    % Positivization是我们自己定义的函数，其作用是进行正向化，其一共接收三个参数
    % 第一个参数是要正向化处理的那一列向量 X(:,Position(i))   回顾上一讲的知识，X(:,n)表示取第n列的全部元素
    % 第二个参数是对应的这一列的指标类型（1：极小型， 2：中间型， 3：区间型）
    % 第三个参数是告诉函数我们正在处理的是原始矩阵中的哪一列
    % 该函数有一个返回值，它返回正向化之后的指标，我们可以将其直接赋值给我们原始要处理的那一列向量
    end
    disp('正向化后的矩阵 X =  ')
    disp(X)
end

Z = X ./ repmat(sum(X.*X) .^ 0.5, n, 1);
disp('标准化矩阵 Z = ')
disp(Z)


D_P = sum([(Z - repmat(max(Z),n,1)) .^ 2 ],2) .^ 0.5;   % D+ 与最大值的距离向量
D_N = sum([(Z - repmat(min(Z),n,1)) .^ 2 ],2) .^ 0.5;   % D- 与最小值的距离向量
S = D_N ./ (D_P+D_N);    % 未归一化的得分
disp('最后的得分为：')
stand_S = S / sum(S)
[sorted_S,index] = sort(stand_S ,'descend')
```