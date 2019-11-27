---
title: Git和Github之间的一些操作
date: 2019-11-26 12:45:55
categories: Git
tags:
---

## 配置SSH

### 什么是SSH

SSH是一种协议标准,其目的是实现安全远程登录以及其它安全网络服务。我们使用SSH就相当于是使用一个身份凭证，通过密钥可以免密登陆。可以将电脑端与Github账号连接起来。

### 配置步骤

1. 获取密钥：`$ ssh-keygen-t rsa-C "your_email@youremail.com"`输入后提示操作路径以及密码，直接回车跳过即可。
2. 从显示路径中获取出密钥：C:\Users\ASUS\.ssh 下的id_rsa.pub文件，打开后复制其中全部内容。
3. 打开Github，找到账户设置中的SSH keys，新建SSH key，随便输入一个title，然后将所有内容粘贴到内容之中。
4. 输入ssh -T git@github.com 可以查看状态。显示 Hi hxk0912! You've successfully authenticated, but GitHub does not provide shell access. 就可以证明绑定成功。


##　上传代码到Github