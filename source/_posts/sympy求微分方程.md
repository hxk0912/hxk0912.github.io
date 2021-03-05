---
title: sympy求微分方程
date: 2021-01-30 18:09:38
categories: sympy
tags:
---

## 解析解

``` python
import sympy
import matplotlib.pyplot as plt
import numpy as np
from scipy import integrate

sympy.init_printing()

alpha,beta,W_max,t = sympy.symbols("alpha,beta,W_max,t")

W = sympy.Function('W')

ode = W(t).diff(t) - (alpha-beta) * W(t) * ((1 - W(t) / W_max))

print(ode)

ode_sol = sympy.dsolve(ode)

print(ode_sol)

ics = {W(0): 10}

C_eq = sympy.Eq(ode_sol.lhs.subs(t, 0).subs(ics), ode_sol.rhs.subs(t, 0))

C_sol = sympy.solve(C_eq)

print(ode_sol.subs(C_sol[0]))

```

