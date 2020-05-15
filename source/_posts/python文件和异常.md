---
title: python文件和异常
date: 2020-05-15 16:06:20
categories: python
tags:
---

## 读写文本文件

``` python
def main():
    f = open('致橡树.txt', 'r',encoding='utf-8')
    print(f.read())
    f.close()

if __name__ == '__main__':
    main()
```

## 异常处理

``` python
def main():
    f = None
    try:
        f = open('致橡树.txt', 'r', encoding='utf-8')
        print(f.read())
    except FileNotFoundError:
        print('无法打开指定的文件!')
    except LookupError:
        print('指定了未知的编码!')
    except UnicodeDecodeError:
        print('读取文件时解码错误!')
    finally:
        if f:
            f.close()


if __name__ == '__main__':
    main()


```