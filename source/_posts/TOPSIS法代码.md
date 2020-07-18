---
title: TOPSIS法代码
date: 2020-07-16 10:06:12
tags:
---

## MATLAB代码

### 清风课代码

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

## python代码

``` python
import numpy as np

import pandas as pd


def PositiveMinToMax(datas):
    return np.max(datas) - datas  # 套公式


# 中间型指标 -> 极大型指标
def PositiveMidToMax(datas, x_best):
    temp_datas = datas - x_best
    M = np.max(abs(temp_datas))
    answer_datas = 1 - abs(datas - x_best) / M  # 套公式
    return answer_datas


# 区间型指标 -> 极大型指标
def PositiveIntToMax(datas, x_min, x_max):
    M = max(x_min - np.min(datas), np.max(datas) - x_max)
    answer_list = []
    for i in datas:
        if i < x_min:
            answer_list.append(1 - (x_min - i) / M)  # 套公式
        elif x_min <= i <= x_max:
            answer_list.append(1)
        else:
            answer_list.append(1 - (i - x_max) / M)
    return np.array(answer_list)


def Standardize(datas, rownum):
    for i in range(0, rownum):
        tmpar = datas[:, i]
        datas[:, i] = tmpar / np.sqrt(sum(tmpar ** 2))
    return datas


def main():
    df = pd.read_csv("20条河流的水质情况数据.csv")

    ans1 = df.values

    m, n = ans1.shape

    ans2 = np.array([ans1[:, 0], PositiveMidToMax(ans1[:, 1], 7), PositiveMinToMax(ans1[:, 2]),
                     PositiveIntToMax(ans1[:, 3], 10, 20)])

    ans3 = np.array(ans2).T

    ans4 = Standardize(ans3, n)

    D_P = np.sqrt(np.power(ans4 - np.tile(ans4.max(axis=0), (m, 1)), 2).sum(axis=1))
    D_N = np.sqrt(np.power(ans4 - np.tile(ans4.min(axis=0), (m, 1)), 2).sum(axis=1))

    S = D_N / (D_P + D_N)

    S_std = S / sum(S)

    sorted_S = np.sort(S_std)[::-1]

    sorted_S = np.expand_dims(sorted_S,axis=1)

    print(sorted_S.shape)

    out_S = np.hstack((ans4,sorted_S))

    print(out_S.shape)

    # np.savetxt("a.csv",out_S,delimiter=',')

    column = ['含氧量（ppm)', 'PH值', '细菌总数(个 / mL)', '植物性营养物量（ppm)', '得分', '熵权得分排序后结果']

    pd_data = pd.DataFrame(out_S, columns=column)

    pd_data.to_csv('b.csv', encoding="gb2312")

main()

```