---
title: C++读写文件
date: 2020-09-04 11:25:33
categories: C++
tags:
---

## 文件的读取与写入

``` C++

#include <iostream>
#include <fstream>

using namespace std;

int main()
{
    ofstream outfile;
    outfile.open("./seq_data.txt");
    int a = 40;
    if(outfile.is_open())
    {
        printf("outfile success!");
        outfile  << a << endl;
    }
    outfile.close();
    return 0;
}


```

## 读取文件

``` C++

#include <iostream>
#include <fstream>

using namespace std;

int main()
{
    ifstream infile;
    infile.open("./seq_data.txt");
    int a = 40;
    int b = 0;
    if(infile.is_open())
    {
        printf("infile success!\n");
        infile >> b ;
        if(b == a)
        {
            cout << "success!"<<endl;
        }
    }
    infile.close();
    return 0;
}


```
