---
title: OpenMV基础使用
date: 2020-09-28 21:26:13
categories: OpenMV
tags:
---

## I/O教程

### LED教程

``` python

import pyb

red_led = pyb.LED(1) # G-2  B-3 IR LEDS-4

while True:
    red_led.on() # off() toggle()
```

### GPIO教程

#### 输入

``` python

import pyb

p = pyb.Pin("P0",pyb.Pin.IN)
p.value() # Return 0 & 1

p = pyb.Pin("P0",pyb.Pin.IN,pyb.Pin.PULL_UP)
```

#### 输出

``` python

import pyb

p = pyb.Pin("P0",pyb.Pin.OUT_PP)
p.high()
p.low()

```

### 模拟I/O

#### ADC

``` python

import pyb

adc = pyb.ADC(pyb.Pin("P6"))

while True:
    pyb.delay(10)
    print("%f vols" % ( adc.read()*3.3)/4095))

```

### UART

``` python

import pyb

uart = pyb.UART(3,115200,timeout_char = 1000)
uart.write("Hello,World!\n")
uart.read()
```

