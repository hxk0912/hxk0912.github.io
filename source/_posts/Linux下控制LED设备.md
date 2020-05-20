---
title: Linux下控制LED设备
date: 2020-05-20 13:49:15
categories: Linux
tags:
---

## 文件操作函数

### 系统调用

Linux提供的文件操作系统调用常用的有open、write、read、lseek、close等。

#### open函数

```C
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
```

返回值是文件的文件描述符

#### read函数

```C
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
```

返回值是实际读取的字节数

#### write函数

```C
#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t count);
```

返回值是实际写入的字节数


#### close函数

```C
int close(int fd);
```

#### lseek函数

```C
off_t lseek(int fd, off_t offset, int whence);
```

它的用法与flseek一样，其中的offset参数用于指定位置，whence参数则定义了offset的意义，whence的可取值如下：

- SEEK_SET：offset是一个绝对位置。
- SEEK_END：offset是以文件尾为参考点的相对位置。
- SEEK_CUR：offset是以当前位置为参考点的相对位置。

返回值是实际写入的字节数

## 控制LED设备代码

``` C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <fcntl.h>

//ARM 开发板LED设备的路径
#define RLED_DEV_PATH "/sys/class/leds/red/brightness"
#define GLED_DEV_PATH "/sys/class/leds/green/brightness"
#define BLED_DEV_PATH "/sys/class/leds/blue/brightness"

int main(int argc, char *argv[])
{
   int res = 0;
   int r_fd, g_fd, b_fd;

   printf("This is the led demo\n");
   //获取红灯的设备文件描述符
   r_fd = open(RLED_DEV_PATH, O_WRONLY);
   if(r_fd < 0){
      printf("Fail to Open %s device\n", RLED_DEV_PATH);
      exit(1);
   }
   //获取绿灯的设备文件描述符
   g_fd = open(GLED_DEV_PATH, O_WRONLY);
   if(g_fd < 0){
      printf("Fail to Open %s device\n", GLED_DEV_PATH);
      exit(1);
   }
   //获取蓝灯的设备文件描述符
   b_fd = open(BLED_DEV_PATH, O_WRONLY);
   if(b_fd < 0){
      printf("Fail to Open %s device\n", BLED_DEV_PATH);
      exit(1);
   }

   while(1){
      //红灯亮
      write(r_fd, "255", 3);
      //延时1s
      sleep(1);
      //红灯灭
      write(r_fd, "0", 1);
   }
}
```