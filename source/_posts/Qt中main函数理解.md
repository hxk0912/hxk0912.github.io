---
title: Qt中main函数理解
date: 2019-11-28 20:35:45
categories: Qt
tags:
---

## main函数内容

``` C++
#include "mainwindow.h"
#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();

    return a.exec();
}
```

## 理解 

1. QApplication 是Qt的标准应用程序类，第一行实例化了一个对象a。
2. 然后定义一个Widget类的变量w用于显示窗口。
3. 定义窗口后使用w.show()显示窗口。
4. 最后返回的a.exec()是应用程序的执行，开始应用程序的消息循环和事件处理。