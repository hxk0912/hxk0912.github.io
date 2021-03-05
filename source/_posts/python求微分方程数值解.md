---
title: python求微分方程数值解
date: 2021-01-30 18:12:52
categories: python
tags:
---

## scipy

``` python
import numpy as np
from scipy.integrate import odeint
import matplotlib.pyplot as plt

alpha = 0.1
beta = 0.09


def func(y, t):
    return 1 * y * ((1-y/15000)**2)

YS=odeint(func,y0=100,t=np.arange(0,50,1))

t=np.arange(0,50,1)
plt.plot(t, YS, label='weight of dragon')
plt.legend()
plt.xlabel("year")
plt.ylabel("weight")
plt.show()
```



