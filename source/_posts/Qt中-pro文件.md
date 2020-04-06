---
title: Qt中.pro文件
date: 2020-04-06 19:46:26
categories: Qt
tags:
---

## 什么是.pro

.pro就是工程文件，它是qmake自动生成的用于生产makefile的配置文件

以下是一个例子：

``` C++
QT       += core gui                            //核心模块添加gui模块

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets //大于Qt4版本时将添加widget模块（为了提高兼容性）

TARGET = MyQtDemo1                              //指定生成的应用程序名

TEMPLATE = app                                  //模版变量告诉qmake生成什么类型的makefile

DEFINES += QT_DEPRECATED_WARNINGS               //定义编译选项。QT_DEPRECATED_WARNINGS表示当Qt的某些功能被标记为过时的，那么编译器会发出警告。

SOURCES += \                                    //源文件
        main.cpp \
        mainwindow.cpp

HEADERS += \                                    //头文件
        mainwindow.h

FORMS += \                                      //界面
        mainwindow.ui

```
