<!--
 * @Descripttion: 
 * @version: 1.0
 * @Author: hxk
 * @Date: 2020-10-24 15:29:20
 * @LastEditors: hxk
 * @LastEditTime: 2020-10-24 15:41:04
-->
---
title: QT中定时器使用
date: 2020-10-24 15:29:20
categories: Qt
tags:
---

## 直接使用

``` C++
id1 = startTimer(1000); //开启一个一秒的定时器，标识符为id1
//重写定时器事件函数
void Widget::timerEvent(QTimerEvent * ev)
{
    if(ev->timerId()==id1)
    {
        //进行想要的操作
    }
}

```

## 使用QTimer类

``` C++
#include <QTimer>

    QTimer *timer = new QTimer(this);

    //启动定时器
    timer->start(500);
    //时间到会发送timeout信号
    connect(timer,&QTimer::timeout,[=](){

        //进行想要的操作

    });

```