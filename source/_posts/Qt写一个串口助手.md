---
title: Qt写一个串口助手
date: 2020-04-16 23:16:33
categories: Qt实战
tags:
---

## 代码

widget.cpp

可以实现基本的功能，收发信息，配置打开串口。

``` C++
#include "widget.h"
#include "ui_widget.h"

Widget::Widget(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::Widget)
{
    ui->setupUi(this);
    InitSerialPort();
    // readyRead就是串口接受到信号，准备读取的状态
    connect(m_SerialPort,&QSerialPort::readyRead,[=](){

        if(m_isOpen)
        {
            QByteArray recv = m_SerialPort->readAll();
            ui->tEditRecv->append(recv);//追加的方式将接收到的数据添加到文本框中
        }

    });
}

Widget::~Widget()
{
    delete ui;
}

bool Widget::InitSerialPort()
{
    //读取可用串口
    QList<QSerialPortInfo> SerialportInfo = QSerialPortInfo::availablePorts();
    //在串口选择框中添加刚才读取到的串口
    for(int i=0;i< SerialportInfo.count();i++)
    {
        ui->cBoxSerialPort->addItem(SerialportInfo.at(i).portName());
    }
    return true;
}

//获取当前配置
void Widget::getCurrentConf(void)
{
    portName=ui->cBoxSerialPort->currentText();//串口名
    baudRate=ui->cBoxBaudRate->currentText(); //波特率
    parity=ui->cBoxParity->currentText();   //校验位
    dataBit=ui->cBoxDataBit->currentText(); //数据位
    stopBit=ui->cBoxStopBit->currentText(); //停止位
}

void Widget::on_btnOpen_clicked()
{
    //m_isopen是串口是否打开的标志位
    if(m_isOpen)
    {
           m_isOpen=false;
           m_SerialPort->close();  //关闭串口
           ui->btnOpen->setText("打开串口");//将按钮文本改为打开串口
           ui->cBoxBaudRate->setEnabled(true);//将上面的各个选项设置为可选状态
           ui->cBoxDataBit->setEnabled(true);
           ui->cBoxParity->setEnabled(true);
           ui->cBoxSerialPort->setEnabled(true);
           ui->cBoxStopBit->setEnabled(true);
    }
    else
    {
           getCurrentConf();    // 打开串口前先获取当前配置
           m_isOpen=true;
           ui->btnOpen->setText("关闭串口"); //将按钮文本改为关闭串口
           ui->cBoxBaudRate->setEnabled(false);//将上面的各个选项设置为不可选状态
           ui->cBoxDataBit->setEnabled(false);
           ui->cBoxParity->setEnabled(false);
           ui->cBoxSerialPort->setEnabled(false);
           ui->cBoxStopBit->setEnabled(false);

           m_SerialPort->setPortName(portName); // 配置串口选项

           if("115200"==baudRate)
           {
               m_SerialPort->setBaudRate(QSerialPort::Baud115200);
           }
           else if("9600"==baudRate)
           {
               m_SerialPort->setBaudRate(QSerialPort::Baud9600);
           }
           else if("4800"==baudRate)
           {
               m_SerialPort->setBaudRate(QSerialPort::Baud4800);
           }

           if("NONE"==parity)
           {
               m_SerialPort->setParity(QSerialPort::NoParity);
           }
           else if("ODD"==parity)
           {
               m_SerialPort->setParity(QSerialPort::OddParity);
           }
           else if("EVEN"==parity)
           {
               m_SerialPort->setParity(QSerialPort::EvenParity);
           }

           if("8"==dataBit)
           {
               m_SerialPort->setDataBits(QSerialPort::Data8);
           }
           else if("7"==dataBit)
           {
               m_SerialPort->setDataBits(QSerialPort::Data7);
           }
           else if("6"==dataBit)
           {
               m_SerialPort->setDataBits(QSerialPort::Data6);
           }
           else if("5"==dataBit)
           {
               m_SerialPort->setDataBits(QSerialPort::Data5);
           }

           if("1"==stopBit)
           {
               m_SerialPort->setStopBits(QSerialPort::OneStop);
           }
           else if("1.5"==stopBit)
           {
               m_SerialPort->setStopBits(QSerialPort::OneAndHalfStop);
           }
           else if("2"==stopBit)
           {
               m_SerialPort->setStopBits(QSerialPort::TwoStop);
           }

           m_SerialPort->open(QSerialPort::ReadWrite);
    }

}

void Widget::on_btnSend_clicked()
{
    if(m_isOpen)
    {
        m_SerialPort->write(ui->tEditSend->toPlainText().toUtf8());//.toutf8将Qstring转为QByteArray
    }
}

```

widget.h

``` C++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QString>
#include <QtSerialPort/QSerialPort>
#include <QtSerialPort/QSerialPortInfo>
#include <QList>

namespace Ui {
class Widget;
}

class Widget : public QWidget
{
    Q_OBJECT

public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();
    QSerialPort * m_SerialPort = new QSerialPort();
    bool InitSerialPort(void);
    void getCurrentConf(void);
    bool m_isOpen=0;
    QString portName;
    QString baudRate;
    QString parity;
    QString dataBit;
    QString stopBit;
private slots:
    void on_btnOpen_clicked();

    void on_btnSend_clicked();

private:
    Ui::Widget *ui;

};



#endif // WIDGET_H

```