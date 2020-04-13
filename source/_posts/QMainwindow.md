---
title: QMainwindow
date: 2020-04-13 13:59:06
categories: QT
tags:
---

## QMainwindow类常用操作

``` C++
#include "mainwindow.h"
#include <QMenuBar>
#include <QToolBar>
#include <QPushButton>

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{
    //重置窗口大小
    resize(600,400);
    //创建一个新的菜单栏(菜单栏至多有一个)
    //菜单栏不用我们创建，menuBar()直接就可以返回menubar指针。
    QMenuBar * mbar = menuBar();
    //将菜单栏放入窗口之中
    setMenuBar(mbar);

    //创建菜单
    QMenu * fileMenu = mbar->addMenu("文件");
    QMenu * editMenu = mbar->addMenu("编辑");

    //创建菜单项
    fileMenu->addAction("新建");
    //创建分隔符
    fileMenu->addSeparator();

    fileMenu->addAction("打开");

    //工具栏（可以有多个）
    QToolBar * tbar = new QToolBar(this);

    //Qt::LeftToolBarArea 枚举变量
    addToolBar(Qt::LeftToolBarArea,tbar);

    //设置工具栏允许停靠的位置
    tbar->setAllowedAreas(Qt::LeftToolBarArea|Qt::RightToolBarArea);

    //设置浮动（只允许停靠在边框处）
    tbar->setFloatable(false);

    //设置移动
    tbar->setMovable(true);

    //创建工具项
    tbar->addAction("新建");
    //创建分隔符
    tbar->addSeparator();

    tbar->addAction("打开");

    //添加控件
    QPushButton * btn = new QPushButton("aa",this);
    tbar->addWidget(btn);

}
```

 
